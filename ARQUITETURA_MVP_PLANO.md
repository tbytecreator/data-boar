# 🏗️ Arquitetura MVP + Plano de Implementação
## Data Boar: Do "Discovery" para "Remediation"

---

## 📐 Visão de Arquitetura Atual vs MVP

### ARQUITETURA ATUAL (v1.7.3)
```
┌─────────────────────────────────────────────────────────┐
│                   OPERADOR (CLI/API)                      │
├─────────────────────────────────────────────────────────┤
│
│  ┌──────────────────┐
│  │  CONFIG.YAML     │ ◄─── Sources, Detection Rules
│  └──────────────────┘
│           │
│           ▼
│  ┌──────────────────────────────────────────────────────┐
│  │         DISCOVERY ENGINE                              │
│  ├──────────────────────────────────────────────────────┤
│  │ • Conectores (SQL, NoSQL, Files, APIs)                │
│  │ • Amostragem inteligente                              │
│  │ • Regex + ML/DL                                       │
│  └──────────────────────────────────────────────────────┘
│           │
│           ▼
│  ┌──────────────────────────────────────────────────────┐
│  │         STORAGE (SQLite)                              │
│  ├──────────────────────────────────────────────────────┤
│  │ • audit_results (findings)                            │
│  │ • Sessions (metadata)                                 │
│  │ • Scan history                                        │
│  └──────────────────────────────────────────────────────┘
│           │
│           ├─────► Excel Report
│           └─────► Heatmap PNG
│
└─────────────────────────────────────────────────────────┘
```

### ARQUITETURA MVP (v2.0 - Proposta)
```
┌─────────────────────────────────────────────────────────┐
│                   OPERADOR (CLI/API)                      │
├─────────────────────────────────────────────────────────┤
│
│  ┌──────────────────┐
│  │  CONFIG.YAML     │ ◄─── Sources, Detection Rules
│  │  + POLICIES.YAML │      + Remediation Policies   ◄─── NEW
│  └──────────────────┘
│           │
│           ▼
│  ┌──────────────────────────────────────────────────────┐
│  │         DISCOVERY ENGINE                              │
│  ├──────────────────────────────────────────────────────┤
│  │ • Conectores (SQL, NoSQL, Files, APIs)                │
│  │ • Amostragem inteligente                              │
│  │ • Regex + ML/DL                                       │
│  │ • Activity Logging (SQL, API calls)              ◄─── NEW
│  └──────────────────────────────────────────────────────┘
│           │
│           ▼
│  ┌──────────────────────────────────────────────────────┐
│  │    POLICY ENGINE (NEW)                                │
│  ├──────────────────────────────────────────────────────┤
│  │ • Evaluate: Achado + Policy → Ação?                   │
│  │ • Approve: Workflow de aprovação                      │
│  │ • Execute: Masking, Delete, Revoke                    │
│  │ • Track: Audit de remediação                          │
│  └──────────────────────────────────────────────────────┘
│           │
│           ▼
│  ┌──────────────────────────────────────────────────────┐
│  │         STORAGE (SQLite + Enhancements)               │
│  ├──────────────────────────────────────────────────────┤
│  │ • audit_results (+ masked_values)                     │
│  │ • Sessions                                            │
│  │ • remediation_log (NEW)                               │
│  │ • activity_log (NEW)                                  │
│  │ • policies (NEW)                                      │
│  │ • alerts (NEW)                                        │
│  └──────────────────────────────────────────────────────┘
│           │
│           ├─────► Excel Report (+ masking)
│           ├─────► Heatmap PNG
│           ├─────► Remediation Dashboard (NEW)
│           ├─────► Activity Monitor (NEW)
│           └─────► Compliance Timeline (NEW)
│
│  ┌──────────────────────────────────────────────────────┐
│  │    INTEGRATIONS (NEW)                                 │
│  ├──────────────────────────────────────────────────────┤
│  │ • Webhook → SIEM (Splunk, Sentinel)                   │
│  │ • Alert Service (email, Slack)                        │
│  │ • Policy Sync (bidirecional)                          │
│  └──────────────────────────────────────────────────────┘
│
└─────────────────────────────────────────────────────────┘
```

---

## 🎯 10 Componentes a Adicionar

### 1️⃣ MASKING ENGINE
```python
# pseudocode
class MaskingEngine:
    def mask_value(self, value: str, pattern: str) -> str:
        if pattern == "CPF":
            return "XXX.XXX.XXX-XX"
        elif pattern == "EMAIL":
            return "xxx@example.com"  # preserve domain
        elif pattern == "CREDIT_CARD":
            return "**** **** **** 1234"  # last 4
        # ... etc
    
    def apply_to_export(self, report_df: DataFrame) -> DataFrame:
        # Apply masking based on detected patterns
        for col, pattern in report_df.schema:
            if is_sensitive(pattern):
                report_df[col] = report_df[col].apply(
                    lambda x: self.mask_value(x, pattern)
                )
        return report_df
```

### 2️⃣ REMEDIATION WORKFLOW ENGINE
```python
class RemediationWorkflow:
    def __init__(self):
        self.workflows = {
            "delete_expired_pii": {
                "trigger": "retention_expired",
                "actions": ["delete_db", "delete_files"],
                "approval": True,
            },
            "revoke_unauthorized_access": {
                "trigger": "unauthorized_access_detected",
                "actions": ["revoke_role", "notify_user"],
                "approval": False,  # immediate
            }
        }
    
    def execute(self, finding: Finding) -> RemediationResult:
        workflow = self.get_workflow(finding)
        if workflow["approval"]:
            result = self.request_approval(finding, workflow)
        else:
            result = self.auto_execute(finding, workflow)
        self.audit_log(result)
        return result
```

### 3️⃣ ACTIVITY MONITORING (DAM)
```python
class ActivityMonitor:
    def __init__(self, sources: Dict):
        self.sources = sources  # SQL logs, API logs, etc
    
    def parse_sql_logs(self, db_type: str, log_path: str) -> List[Activity]:
        """Parse SQL logs and extract queries touching sensitive data"""
        activities = []
        if db_type == "postgresql":
            activities = self.parse_postgresql_logs(log_path)
        elif db_type == "mysql":
            activities = self.parse_mysql_logs(log_path)
        # Filter for sensitive table access
        return [a for a in activities if self.is_sensitive_table(a.table)]
    
    def detect_anomaly(self, activity: Activity) -> bool:
        """Detect unusual access patterns"""
        baseline = self.get_baseline_for_user(activity.user)
        return activity.count > baseline * 3  # 3x normal usage
```

### 4️⃣ ACCESS CONTROL ENGINE
```python
class AccessControlEngine:
    def __init__(self, policy_file: str):
        self.policies = self.load_policies(policy_file)
    
    def can_user_access(self, user: str, table: str, column: str) -> bool:
        """Evaluate if user has access to column"""
        user_role = self.get_user_role(user)
        required_perm = f"access:{table}:{column}"
        return required_perm in self.policies[user_role]
    
    def enforce_on_query(self, query: str, user: str) -> str:
        """Rewrite query to enforce column-level access"""
        # Example: SELECT * FROM users 
        # → SELECT id, name FROM users (remove email, phone)
        cols_allowed = self.get_allowed_columns(user, "users")
        return f"SELECT {cols_allowed} FROM users WHERE ..."
```

### 5️⃣ REAL-TIME ALERTS
```python
class AlertService:
    def __init__(self, config: Dict):
        self.webhooks = config.get("webhooks", [])
        self.email_recipients = config.get("email", [])
    
    def on_new_sensitive_data(self, finding: Finding):
        """Triggered when new PII is discovered"""
        alert = Alert(
            severity="HIGH",
            message=f"New {finding.pattern} discovered in {finding.source}",
            finding=finding,
            timestamp=now()
        )
        self.send_webhooks(alert)
        self.send_email(alert)
        self.store_alert(alert)
    
    def send_webhooks(self, alert: Alert):
        for webhook in self.webhooks:
            requests.post(webhook["url"], json=alert.to_dict())
```

### 6️⃣ DATA LINEAGE TRACKER
```python
class LineageTracker:
    def __init__(self, db: Database):
        self.db = db
    
    def track_data_flow(self, finding: Finding) -> Lineage:
        """Build data genealogy: source → transformation → destination"""
        lineage = Lineage(source=finding.source)
        
        # 1. Find ETL that reads from source
        etl_reads = self.find_etl_reads(finding.source)
        
        # 2. For each ETL, find what it writes
        for etl in etl_reads:
            lineage.add_transformation(etl)
            destinations = self.find_etl_writes(etl)
            for dest in destinations:
                lineage.add_destination(dest)
        
        return lineage
```

### 7️⃣ SIEM INTEGRATION
```python
class SIEMConnector:
    def __init__(self, siem_type: str, config: Dict):
        self.type = siem_type  # splunk, sentinel, etc
        self.config = config
    
    def send_finding(self, finding: Finding):
        """Convert finding to SIEM event"""
        if self.type == "splunk":
            self.send_to_splunk(self.convert_to_splunk_event(finding))
        elif self.type == "sentinel":
            self.send_to_sentinel(self.convert_to_sentinel_alert(finding))
    
    def convert_to_splunk_event(self, finding: Finding) -> Dict:
        return {
            "event_type": "data_discovery",
            "source": finding.source,
            "sensitive_pattern": finding.pattern,
            "count": finding.count,
            "severity": "high",
            "timestamp": now(),
        }
```

### 8️⃣ RBAC ENGINE (Granular)
```python
class GranularRBAC:
    def __init__(self, policy_file: str):
        self.roles = self.load_roles(policy_file)
    
    def can_view_column(self, user: str, column: str) -> bool:
        """Check if user can see specific column in reports"""
        user_role = self.get_user_role(user)
        allowed_cols = self.roles[user_role].get("visible_columns", [])
        return column in allowed_cols
    
    def filter_report_for_user(self, report_df: DataFrame, user: str) -> DataFrame:
        """Remove columns user doesn't have access to"""
        allowed_cols = self.get_user_allowed_columns(user)
        return report_df[[c for c in report_df.columns if c in allowed_cols]]
```

### 9️⃣ COMPLIANCE AUTOMATION
```python
class ComplianceAutomation:
    def __init__(self, frameworks: Dict):
        self.frameworks = frameworks  # LGPD, GDPR, etc
    
    def auto_delete_on_retention_expired(self, finding: Finding):
        """LGPD Art. 18: Auto-delete after retention period"""
        retention_days = self.get_retention_for_finding(finding)
        if finding.age_days > retention_days:
            result = self.delete_finding(finding)
            self.log_dsar_fulfillment(finding, result)
    
    def generate_dsar_report(self, user_email: str) -> Report:
        """Generate Data Subject Access Request"""
        findings = self.find_all_for_user(user_email)
        return Report(
            user=user_email,
            findings=findings,
            generated_at=now(),
            export_format="csv+json"
        )
```

### 🔟 AUDIT & VERSIONING
```python
class AuditEngine:
    def __init__(self, db: Database):
        self.db = db
    
    def track_change(self, finding: Finding, change_type: str, details: Dict):
        """Log every finding change"""
        audit_record = AuditRecord(
            finding_id=finding.id,
            change_type=change_type,  # discovered, masked, deleted, etc
            details=details,
            timestamp=now(),
            user=get_current_user(),
        )
        self.db.audit_log.insert(audit_record)
    
    def get_finding_timeline(self, finding_id: str) -> List[AuditRecord]:
        """Show evolution of a finding across scans"""
        return self.db.audit_log.filter(finding_id=finding_id).order_by("timestamp")
```

---

## 📋 Especificação YAML para MVP

### `config.yaml` (Expandido)
```yaml
# Existing config...
targets:
  - type: postgresql
    # ...

detection:
  # Existing...
  masking:
    enabled: true
    rules:
      - pattern_name: "CPF"
        type: "partial"  # xxx.xxx.xxx-xx
      - pattern_name: "EMAIL"
        type: "domain"   # ***@example.com
      - pattern_name: "CREDIT_CARD"
        type: "last_four" # **** **** **** 1234

# NEW: Remediation Policies
remediation:
  enabled: true
  policies:
    - id: "delete_expired_pii"
      name: "Delete Expired PII"
      trigger:
        type: "retention_expired"
        days: 30
      actions:
        - type: "delete"
          scope: ["database", "files"]
        - type: "notify"
          recipients: ["dpo@company.com"]
      approval_required: true
      
    - id: "mask_in_dev"
      name: "Auto-mask in Development"
      trigger:
        type: "environment"
        value: "development"
      actions:
        - type: "mask"
          scope: ["database"]
          in_place: true
      approval_required: false

# NEW: Activity Monitoring
monitoring:
  enabled: true
  sources:
    - type: "postgresql_logs"
      path: "/var/log/postgresql.log"
      parse_queries: true
    - type: "mysql_logs"
      path: "/var/log/mysql-slow.log"
  
  alerts:
    - trigger: "sensitive_table_access"
      threshold: 3  # 3x baseline
      action: "notify"

# NEW: Access Control
access_control:
  enabled: true
  rbac:
    dpo:
      visible_columns: "*"  # All
    security:
      visible_columns: ["finding", "pattern", "severity"]
    analyst:
      visible_columns: ["finding", "pattern", "count"]

# NEW: Integrations
integrations:
  siem:
    - type: "splunk"
      url: "https://splunk.company.com"
      token: "${SPLUNK_HEC_TOKEN}"
  
  alerts:
    webhooks:
      - url: "https://webhook.example.com/data-boar"
        events: ["new_finding", "remediation_failed"]
    email:
      recipients: ["dpo@company.com", "security@company.com"]
      on_events: ["high_severity_finding"]
```

---

## 📅 Roadmap Detalhado (90 dias)

### SPRINT 1 (Semanas 1-2): Foundation

```ascii
Week 1:
├─ Day 1-2:   Setup dev env, planning, architecture review
├─ Day 3-4:   Implement Masking Engine (3d effort)
├─ Day 5:     Unit tests + docs
└─ End: PR review, merge to main

Week 2:
├─ Day 1-2:   Implement Remediation Workflow (basic) (3d effort)
├─ Day 3-4:   Approval workflow (simple)
├─ Day 5:     Integration tests
└─ End: PR review, beta test with early customer
```

### SPRINT 2 (Semanas 3-4): Automation

```ascii
Week 3:
├─ Day 1-2:   Compliance Automation (DSAR, retention) (3d effort)
├─ Day 3-4:   Scheduling engine
├─ Day 5:     Integration with Masking
└─ End: Internal demo

Week 4:
├─ Day 1-2:   Activity Monitoring (DAM basics) (3d effort)
├─ Day 3-4:   SQL log parsing
├─ Day 5:     Testing, docs
└─ End: Release Sprint 2 to prod (beta channel)
```

### SPRINT 3 (Semanas 5-6): Visibility

```ascii
Week 5:
├─ Day 1-2:   Alerting System (2d effort)
├─ Day 3-4:   Webhook + email
├─ Day 5:     Dashboard for alerts
└─ End: Demo to stakeholders

Week 6:
├─ Day 1-2:   SIEM Integration (Splunk + Sentinel) (3d effort)
├─ Day 3-4:   Event mapping, playbooks
├─ Day 5:     Full integration test
└─ End: Release Sprint 3 to prod
```

### SPRINT 4 (Semanas 7-8): Control

```ascii
Week 7:
├─ Day 1-2:   Access Control Engine (3d effort)
├─ Day 3-4:   RBAC enforcement
├─ Day 5:     Dashboard filtering
└─ End: QA testing

Week 8:
├─ Day 1-2:   Data Lineage (basic) (3d effort)
├─ Day 3-4:   UI for lineage graph
├─ Day 5:     Perf testing
└─ End: Release Sprint 4
```

### SPRINT 5 (Semanas 9-10): Polish

```ascii
Week 9:
├─ Day 1-2:   Versioning + Timeline UI (2d effort)
├─ Day 3-4:   Audit trail improvements
├─ Day 5:     Edge case testing
└─ End: Customer UAT

Week 10:
├─ Day 1-2:   Bug fixes + perf tuning
├─ Day 3-4:   Documentation + runbooks
├─ Day 5:     Security audit
└─ End: Release Candidate
```

### SPRINT 6 (Semanas 11-13): Release

```ascii
Week 11:
├─ Day 1-2:   Final QA + staging
├─ Day 3-4:   Load testing
├─ Day 5:     Migration guide
└─ End: Go/no-go decision

Week 12:
├─ Day 1:     Production deployment
├─ Day 2-3:   Monitoring + support
├─ Day 4-5:   Customer onboarding
└─ End: GA announcement

Week 13:
├─ Day 1-2:   Feedback collection
├─ Day 3-5:   Post-release hotfixes
└─ End: v2.0 Stable
```

---

## 🚀 Equipe + Recursos

### Tamanho de Equipe (90 dias)

- **1 Tech Lead** (architecture, code review)
- **2 Backend Engineers** (implementation)
- **1 Frontend Engineer** (dashboard UI)
- **1 QA Engineer** (testing)
- **0.5 DevOps Engineer** (deployment, infra)
- **0.5 Product Manager** (roadmap, customer feedback)

**Total:** ~3.5 FTE

### Recursos Necessários

- **Dev Environment:** Docker + Kubernetes (local)
- **CI/CD:** GitHub Actions (already using)
- **Testing:** Jest, Pytest (existing)
- **Monitoring:** Datadog/Prometheus (optional, not critical for MVP)
- **Cloud:** AWS EC2 (for staging/beta) - $500-1000/month

### Custos Estimados

- **Engenharia:** 3.5 FTE × $150k/year × 0.25 (90 days) = ~$131k
- **Infra/Tooling:** ~$2k
- **Customer Success (onboarding):** ~$10k
- **Total:** ~$143k

---

## ✅ Success Criteria (MVP v2.0)

### Funcionalidades

- [x] Data Masking em todos exports
- [x] Remediação automática (1 workflow: delete)
- [x] Activity logging básico
- [x] Alertas em tempo real
- [x] SIEM integration (Splunk OU Sentinel)
- [x] RBAC no dashboard
- [x] Compliance automation (DSAR, retention)
- [x] Data lineage MVP
- [x] Audit trail completo
- [x] Versionamento de mudanças

### Qualidade

- [x] >80% test coverage (backend)
- [x] Performance: scan 1M records em <30min
- [x] Uptime: 99.5% no staging
- [x] Security: penetration test clean
- [x] Documentation: complete + runbooks

### Business

- [x] 3+ early customer pilots completed
- [x] NPS >50
- [x] 0 critical bugs on release day
- [x] Competitive pricing set ($10k-$50k/year)
- [x] Sales collateral ready

---

## 📞 Próximos Passos

### Imediato (Esta Semana)

1. [ ] Review de arquitetura com engineering
2. [ ] Kick-off meeting com stakeholders
3. [ ] Setup de infraestrutura (dev env)
4. [ ] Planejamento detalhadode Sprint 1

### Curto Prazo (Próximas 2 Semanas)

1. [ ] Sprint 1 sprint planning
2. [ ] Design docs para Masking + Remediation
3. [ ] Identificar early pilot customers (2-3)
4. [ ] Setup de test environment

### Médio Prazo (Próximas 4 Semanas)

1. [ ] Sprint 1 release (beta)
2. [ ] Customer feedback loops
3. [ ] Sprint 2 planning
4. [ ] Comunicação interna (roadmap)

---

## 📚 Documentação de Referência

- [ANALISE_COMPETITIVA_MVP.md](./ANALISE_COMPETITIVA_MVP.md) - Análise completa
- [MATRIZ_FEATURES_DETALHADA.md](./MATRIZ_FEATURES_DETALHADA.md) - Feature matrix
- [SUMARIO_EXECUTIVO_MVP.md](./SUMARIO_EXECUTIVO_MVP.md) - Executive summary
- [docs/TECH_GUIDE.pt_BR.md](./docs/TECH_GUIDE.pt_BR.md) - Technical foundation
- [docs/USAGE.pt_BR.md](./docs/USAGE.pt_BR.md) - Current API/CLI docs

---

**Documento:** Arquitetura MVP + Plano de Implementação  
**Data:** 2 de maio de 2026  
**Versão:** 1.0  
**Status:** Pronto para Review com Engineering