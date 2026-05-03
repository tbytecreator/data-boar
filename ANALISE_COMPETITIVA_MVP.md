# Análise Competitiva: Data Boar vs Concorrentes

## Funcionalidades para MVP (Produto Mínimo Viável)

**Data:** 2 de maio de 2026  
**Autor:** Análise Comparativa - Data Boar  
**Status:** Recomendação Estratégica para MVP

---

## Resumo Executivo

O **Data Boar** possui excelente posicionamento em **descoberta de dados** e **classificação de sensibilidade** orientada a conformidade. Comparado aos 5 principais concorrentes, o produto é especialmente forte em:

- ✅ Multi-dialeto de fontes (SQL, NoSQL, APIs, arquivos, Power BI, Dataverse, SharePoint)
- ✅ Configuração orientada a conformidade (LGPD, GDPR, CCPA, etc.)
- ✅ Detecção de dados de menores/crianças como prioridade
- ✅ Relatórios executivos e heatmaps
- ✅ Amostragem eficiente para grandes volumes

**Porém**, para ser um **MVP competitivo**, o Data Boar precisa de **10 funcionalidades críticas** que os concorrentes já oferecem em seus core products.

---

## 1. Concorrentes Identificados

| Concorrente           | Posicionamento                            | Público-alvo          | Nível     |
|-----------------------|-------------------------------------------|-----------------------|-----------|
| **BigID**             | DSPM + DLP + Privacy Automation           | Enterprise            | Premium   |
| **Collibra**          | Data Governance + Catalog + Quality       | Enterprise/Mid-market | Premium   |
| **Varonis**           | DSPM + DAM + Insider Risk                 | Enterprise            | Premium   |
| **Microsoft Purview** | Data Classification + DLP (Microsoft 365) | Microsoft-centric     | Enterprise|
| **Imperva**           | Data Security + DLP + App Security        | Enterprise            | Premium   |

---

## 2. Funcionalidades Atuais do Data Boar

### ✅ Forças (Core Product)

1. **Descoberta Multi-Dialeto** (SQL, NoSQL, APIs, arquivos, SharePoint, Power BI, Dataverse)
2. **Classificação de Sensibilidade** (PII, dados sensíveis conforme LGPD Art. 5, GDPR Art. 9)
3. **Detecção Determinística** (Regex + ML/DL opcional)
4. **Suporte a Múltiplos Frameworks** (LGPD, GDPR, CCPA, GLBA, UK GDPR, PIPEDA, POPIA, APPI, etc.)
5. **Detecção de Dados de Menores** (Prioridade de produto, não rodapé)
6. **Relatórios Excel + Heatmaps** (Executivos + técnicos)
7. **API REST + Dashboard Web** (CLI + HTTP)
8. **Amostragem Configurável** (Timeouts por motor)
9. **OCR + Análise de Mídia Rica** (Imagens, legendas)
10. **Configuração YAML** (Extensível, não requer código)

### ⚠️ Gaps Críticos (vs. Concorrentes)

---

## 3. Funcionalidades Críticas Faltando para MVP

### 🔴 CRÍTICA #1: Data Masking / Redaction

**Status:** ❌ Não possui  
**Concorrentes com:** BigID, Varonis, Imperva, Purview  

**O que faz:** Oculta dados sensíveis em exportações e dashboards (PII → `***`, CPF → `XXX.XXX.XXX-XX`)  
**Por que é crítica:** Usuários finais precisam ver contexto mas não o valor real do PII  
**Impacto MVP:** SEM ISSO, dashboard expõe PII em tela → compliance risk  
**Esforço estimado:** Médio (3-5 dias: templates + lógica de apply)

---

### 🔴 CRÍTICA #2: Remediação Automatizada

**Status:** ❌ Não possui  
**Concorrentes com:** BigID, Varonis (ambos executam remediação automática)  

**O que faz:** Aplicar ações automáticas (masking, acesso revogado, data deletion)  
**Por que é crítica:** MVP deve permanecer além de "encontrar problema" → "ajudar a resolver"  
**Impacto MVP:** Hoje Data Boar é relatório; precisa de "enforce policies"  
**Esforço estimado:** Alto (7-10 dias: workflows, rollback, RBAC)

---

### 🔴 CRÍTICA #3: Data Activity Monitoring (DAM)

**Status:** ⚠️ Parcial (não monitora logs de SQL em tempo real)  
**Concorrentes com:** Varonis, BigID, Imperva  

**O que faz:** Detectar QUEM acessou QUAIS dados QUANDO (audit trail de queries/API)  
**Por que é crítica:** Conformidade exige rastreamento de acesso (LGPD Art. 37, GDPR Art. 32)  
**Impacto MVP:** Sem DAM, não há evidência de quem tocou dado sensível  
**Esforço estimado:** Alto (10-15 dias: conectores de log, parsing, alertas)

---

### 🔴 CRÍTICA #4: Access Control / Governance

**Status:** ❌ Não possui  
**Concorrentes com:** BigID (DAG), Varonis, Purview, Collibra  

**O que faz:** Gerenciar quem pode acessar quais dados (RBAC granular por coluna/tabela)  
**Por que é crítica:** Data Boar descobre sensibilidade; precisa LIMITAR acesso  
**Impacto MVP:** MVP fica incompleto: "descobri dados sensíveis, agora e daí?"  
**Esforço estimado:** Alto (10-12 dias: policy engine, sync com fonte, RBAC)

---

### 🔴 CRÍTICA #5: Remediação de Conformidade (Compliance Remediation)

**Status:** ⚠️ Parcial (recomendações em Excel, não enforcement)  
**Concorrentes com:** BigID, Varonis, Purview  

**O que faz:** Aplicar automaticamente controles para cumprir regulamento (ex.: deletar PII após retenção)  
**Por que é crítica:** LGPD Art. 18 (direito à exclusão), GDPR Art. 17 (direito ao esquecimento)  
**Impacto MVP:** MVP precisa demonstrar "conformidade acionável", não apenas "descoberta"  
**Esforço estimado:** Médio-Alto (8-10 dias: scheduling, approval workflows)

---

### 🔴 CRÍTICA #6: Data Lineage / Genealogia

**Status:** ❌ Não possui  
**Concorrentes com:** Collibra (forte), BigID, Purview, Varonis  

**O que faz:** Rastrear origem → transformação → destino do dado (ETL trails)  
**Por que é crítica:** ROPA (Record of Processing Activities) obrigatório em GDPR/LGPD  
**Impacto MVP:** DPO não consegue responder "para onde foi esse CPF?"  
**Esforço estimado:** Alto (12-15 dias: DAG de pipelines, parser de logs)

---

### 🟠 CRÍTICA #7: Integração com Controles Externos (SIEM/DLP)

**Status:** ⚠️ Mínima (API aberta, mas sem playbooks pré-feitos)  
**Concorrentes com:** BigID, Varonis (integram com Splunk, Sentinel, Palo Alto)  

**O que faz:** Enviar achados para SIEM/DLP/Firewall aplicar políticas  
**Por que é crítica:** Empresa típica já tem SIEM; Data Boar precisa se integrar  
**Impacto MVP:** MVP pode ficar isolado; precisa de bridge  
**Esforço estimado:** Médio (5-7 dias: webhooks + mapping, docs)

---

### 🟠 CRÍTICA #8: RBAC Avançado (por Recurso/Coluna)

**Status:** ⚠️ Parcial (RBAC por usuário/rota, não por dado)  
**Concorrentes com:** Purview, BigID (RBAC granular por coluna/tabela)  

**O que faz:** Usuários veem apenas relatórios de dados que tem acesso  
**Por que é crítica:** Grandes orgs têm múltiplas unidades; cada vê só "seu" escopo  
**Impacto MVP:** MVP pode expor relatório de dados sensíveis a usuário sem permissão  
**Esforço estimado:** Médio (6-8 dias: policy evaluation, row-level filtering)

---

### 🟠 CRÍTICA #9: Alertas em Tempo Real

**Status:** ❌ Não possui  
**Concorrentes com:** Varonis, BigID (alertam de novo PII detectado, mudança de acesso)  

**O que faz:** Notificação automática quando nova sensibilidade é descoberta  
**Por que é crítica:** MVP deve ser "sistema de alerta", não apenas "relatório pós-scan"  
**Impacto MVP:** SEM ISSO, descoberta é reativa (espera próximo scan)  
**Esforço estimado:** Médio (5-7 dias: scheduler + notificação)

---

### 🟠 CRÍTICA #10: Versionamento e Rastreamento de Mudanças

**Status:** ⚠️ Parcial (banco SQLite com histórico, sem UI)  
**Concorrentes com:** BigID, Collibra, Varonis (UI mostra evolution de sensibilidade)  

**O que faz:** Visualizar como dado "evoluiu" entre scans (coluna virou sensível? acessos mudaram?)  
**Por que é crítica:** Compliance exige "auditoria de mudanças" (LGPD Art. 37)  
**Impacto MVP:** MVP precisa demonstrar "quais alterações ocorreram"  
**Esforço estimado:** Médio (6-8 dias: diff engine, UI timeline)

---

## 4. Tabela Comparativa Detalhada

| Funcionalidade                    | Data Boar | BigID | Collibra | Varonis | Purview | Imperva |
|-----------------------------------|---|---|---|---|---|---|
| **Descoberta de Dados**           | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Classificação PII**             | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **ML/DL para Sensibilidade**      | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Múltiplos Frameworks**          | ✅ | ✅ | ⚠️ | ⚠️ | ⚠️ | ⚠️ |
| **Detecção de Menores**           | ✅ | ⚠️ | ❌ | ❌ | ❌ | ❌ |
| **Data Masking**                  | ❌ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Remediação Automática**         | ❌ | ✅ | ✅ | ✅ | ⚠️ | ⚠️ |
| **DAM (Activity Monitoring)**     | ❌ | ✅ | ❌ | ✅ | ⚠️ | ✅ |
| **Access Governance**             | ❌ | ✅ | ✅ | ✅ | ✅ | ⚠️ |
| **DSPM (Posture Mgmt)**           | ❌ | ✅ | ⚠️ | ✅ | ✅ | ❌ |
| **Data Lineage**                  | ❌ | ⚠️ | ✅ | ⚠️ | ⚠️ | ❌ |
| **Integração SIEM**               | ⚠️ | ✅ | ⚠️ | ✅ | ✅ | ✅ |
| **RBAC Granular**                 | ⚠️ | ✅ | ✅ | ✅ | ✅ | ⚠️ |
| **Alertas em Tempo Real**         | ❌ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Compliance Reporting**          | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Versionamento de Mudanças**     | ⚠️ | ✅ | ✅ | ✅ | ✅ | ⚠️ |
| **DLP (Data Loss Prevention)**    | ❌ | ✅ | ⚠️ | ✅ | ✅ | ✅ |
| **Suporte a Cloud Nativo**        | ⚠️ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **API REST**                      | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Dashboard Web**                 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |

**Legenda:**

- ✅ = Possui nativamente
- ⚠️ = Possui parcialmente ou em preview
- ❌ = Não possui

---

## 5. Recomendações para MVP (Priorização)

### Fase 1 (Sprint 1-2): Críticas Imediatas

**Objetivo:** Bloquear blockers de conformidade regulatória

#### 1. **Data Masking** (Dias 1-5)

- Implementar templates de masking (PII, CPF, email, etc.)
- Aplicar em Excel exports e dashboard
- Adicionar config em `detection.masking_rules`

#### 2. **Remediação Básica** (Dias 6-10)

- Workflow simples de aprovação (Aprovar → Deletar PII marcado)
- Integração com cada conectador (SQL DELETE, file rm, etc.)
- Log de remediações em audit table

#### 3. **Compliance Remediation Workflow** (Dias 11-15)

- Agendador de retenção: deletar dados > X dias
- DSAR automation: gerar export de DSAR automaticamente
- Acompanhamento de conformidade (checklist LGPD/GDPR)

### Fase 2 (Sprint 3-4): Visibilidade Operacional

**Objetivo:** Dashboard executivo para "o que mudou"

#### 4. **Data Activity Monitoring Básico** (Dias 16-25)

- Parser de query logs (PostgreSQL, MySQL, SQL Server)
- Relatório simples: "quem acessou qual tabela quando"
- Alertas de acesso a dados sensíveis

#### 5. **Alertas em Tempo Real** (Dias 26-30)

- Scheduler de scan incremental (diário)
- Webhook/email alertando nova sensibilidade
- SIEM integration (Splunk, Sentinel)

### Fase 3 (Sprint 5+): Controle Avançado

**Objetivo:** Governance e automação

#### 6. **Access Control Engine** (Dias 31-45)

- Policy definition por framework (LGPD exige role-based access)
- Enforce via proxy/connector
- RBAC granular por coluna

#### 7. **Data Lineage** (Dias 46-60)

- Ingesta de logs de ETL (Talend, SSIS, Airflow)
- Grafo de dados (origem → transformação → destino)
- ROPA mapping automático

#### 8. **Versionamento de Mudanças UI** (Dias 61-70)

- Timeline visual de sensibilidade
- Comparação entre scans
- Audit trail completo

#### 9. **SIEM/DLP Integração Profunda** (Dias 71-80)

- Playbooks pré-feitos (Splunk, Sentinel, Palo Alto)
- Sync de políticas bidirecional
- Enforcement orquestrado

#### 10. **RBAC por Recurso** (Dias 81-90)

- Filtro de relatório por usuário
- Dashboard mostra apenas "seu escopo"
- Multi-tenant ready

---

## 6. Matriz de Esforço vs Impacto

| Funcionalidade | Esforço | Impacto | Prioridade | Sprint |
|---|---|---|---|---|
| Data Masking | Médio (3-5d) | 🔴 Alto | 1️⃣ | 1 |
| Remediação Básica | Alto (7-10d) | 🔴 Alto | 1️⃣ | 1-2 |
| Compliance Workflow | Médio (8-10d) | 🔴 Alto | 1️⃣ | 2 |
| DAM Básico | Alto (10-15d) | 🟠 Médio | 2️⃣ | 3-4 |
| Alertas em Tempo Real | Médio (5-7d) | 🟠 Médio | 2️⃣ | 4 |
| Data Lineage | Alto (12-15d) | 🟠 Médio | 3️⃣ | 5-6 |
| SIEM Integration | Médio (5-7d) | 🟠 Médio | 2️⃣ | 4 |
| RBAC Avançado | Médio (6-8d) | 🟡 Baixo | 3️⃣ | 5 |
| Versionamento UI | Médio (6-8d) | 🟡 Baixo | 3️⃣ | 5 |
| Access Control | Alto (10-12d) | 🔴 Alto | 1️⃣ | 2-3 |

---

## 7. Roadmap Recomendado para MVP (90 dias)

```
SEMANA 1-2 (Sprint 1):
├── ✅ Data Masking (templates básicas)
├── ✅ Remediação básica (aprovação + delete)
└── ✅ Compliance checklist

SEMANA 3-4 (Sprint 2):
├── ✅ Compliance Remediation Workflow
├── ✅ DSAR Automation
└── ✅ Retenção agendada

SEMANA 5-6 (Sprint 3):
├── ✅ DAM - SQL Query Logging
├── ✅ Alertas em Tempo Real
└── ✅ Webhook para SIEM

SEMANA 7-8 (Sprint 4):
├── ✅ Integração Splunk/Sentinel
├── ✅ Playbooks pré-feitos
└── ✅ RBAC básico dashboard

SEMANA 9-10 (Sprint 5):
├── ✅ Data Lineage (MVP)
├── ✅ Timeline visual
└── ✅ Audit trail completo

SEMANA 11-12 (Sprint 6):
├── ✅ Versionamento UI
├── ✅ RBAC granular por coluna
└── ✅ Multi-tenant ready

SEMANA 13 (Release):
└── ✅ MVP v2.0 - Production Ready
```

---

## 8. Especificação Técnica Resumida

### Data Masking

```yaml
detection:
  masking_rules:
    - pattern_name: "EMAIL"
      mask_type: "partial"  # xxx@example.com
    - pattern_name: "CPF"
      mask_type: "last_digits"  # XXX.XXX.XXX-XX
    - pattern_name: "CREDIT_CARD"
      mask_type: "full"  # ****-****-****-****
```

### Remediação Básica

```yaml
remediation:
  workflows:
    - id: "delete_expired_pii"
      trigger: "retention_expired"
      actions:
        - type: "delete"
          scope: "database"
        - type: "delete"
          scope: "file"
      approval_required: true
      audit_log: true
```

### DAM (Activity Monitoring)

```yaml
monitoring:
  sources:
    - type: "sql_logs"
      target: "PostgreSQL"
      parse_queries: true
      alert_on_sensitive_access: true
```

### Alertas em Tempo Real

```yaml
alerts:
  enabled: true
  channels:
    - type: "webhook"
      url: "https://siem.example.com/webhook"
    - type: "email"
      recipients: ["dpo@example.com"]
  triggers:
    - new_sensitive_data
    - unauthorized_access
    - retention_expired
```

---

## 9. Benefícios Esperados (MVP + 10 Features)

| Aspecto | Antes | Depois |
|---|---|---|
| **Descoberta** | ✅ Manual/Script | ✅ Automática + Alertas |
| **Conformidade** | ⚠️ Relatório passivo | ✅ Ativa + Automação |
| **Remediação** | ❌ Manual | ✅ Automática com aprovação |
| **Monitoramento** | ❌ Sem visibility | ✅ DAM em tempo real |
| **Governança** | ❌ Ad-hoc | ✅ Policy-driven |
| **Integração** | ⚠️ API crua | ✅ SIEM + playbooks |
| **Postura Regulatória** | ⚠️ "Esperamos auditoria" | ✅ "Estamos conformes" |

---

## 10. Conclusão

**Data Boar** é uma **ferramenta especializada e forte** em descoberta de dados e classificação orientada a conformidade. Para se tornar um **MVP competitivo**, precisa adicionar:

### Top 3 Críticas (MVP-must):

1. **Data Masking** → Protege PII em tela
2. **Remediação Automática** → Converte descoberta em ação
3. **Access Control** → Governa quem acessa sensíveis

### Top 3 Secundárias (MVP-strong):]

4. **DAM** → Auditoria de quem acessou
5. **Alertas Tempo Real** → Descobre mudanças imediatamente
6. **SIEM Integration** → Integra com stack de segurança

### Estimativa Total:

- **Esforço:** ~90 dias (team de 2-3 eng)
- **Custo:** ~3-4 engenheiros sênior
- **ROI:** Produto viável para venda/suporte

**Recomendação:** Priorizar Fase 1 (semanas 1-4) como MVP 1.0 core, e Fase 2-3 como roadmap de "extensão pós-release".

---

## Anexos

### A. Mapeamento de Concorrentes por Strengths

| Força | BigID | Collibra | Varonis | Purview | Imperva |
|---|---|---|---|---|---|
| Descoberta | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ |
| Governança | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ |
| Remediação | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ |
| Security | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| Compliance | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| AI/ML | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐ |
| Integração | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ |

### B. Preço Estimado dos Concorrentes (2026)

| Solução | Modelo | Faixa | Audiência |
|---|---|---|---|
| **BigID** | Usuários + GB scanned | $50k-$200k+ / ano | Enterprise |
| **Collibra** | Usuários + nodes | $40k-$150k+ / ano | Enterprise/Mid |
| **Varonis** | Usuários + fontes | $60k-$250k+ / ano | Enterprise |
| **Purview** | M365 + add-on DLP | $5-$30/user/mês | M365 orgs |
| **Imperva** | Throughput/queries | $80k-$300k+ / ano | Enterprise |
| **Data Boar MVP** | TBD (target: $10k-$30k/ano) | Mid-market | Orgs LGPD/GDPR |

---

**Documento preparado para:** Stakeholders, Product Team, Engineering Lead  
**Próximos passos:** Review + Sprint Planning + Kick-off