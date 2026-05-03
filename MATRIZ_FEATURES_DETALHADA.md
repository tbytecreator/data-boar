# 🎯 Matriz de Funcionalidades Detalhada
## Data Boar vs. 5 Concorrentes - MVP Comparison

---

## Legenda Rápida
| Símbolo | Significado |
|---------|------------|
| ✅ | Possui nativamente, production-ready |
| ⚠️ | Possui parcialmente ou em preview/beta |
| ❌ | Não possui |
| 🏆 | Força diferenciadora |

---

## 1. DESCOBERTA DE DADOS

### Conectores & Fontes
| Fonte | Data Boar | BigID | Collibra | Varonis | Purview | Imperva |
|------|---------|-------|---------|---------|---------|---------|
| **SQL (PostgreSQL, MySQL, MSSQL, Oracle)** | ✅ | ✅ | ✅ | ✅ | ✅ | ⚠️ |
| **NoSQL (MongoDB, Cassandra, DynamoDB)** | ✅ | ✅ | ⚠️ | ✅ | ⚠️ | ❌ |
| **Data Warehouses (Snowflake, BigQuery)** | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ |
| **File Systems (SFTP, NFS, SMB)** | ✅ | ✅ | ⚠️ | ✅ | ✅ | ✅ |
| **Cloud Storage (S3, Azure Blob, GCS)** | ⚠️ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **APIs & REST** | ✅ | ✅ | ⚠️ | ✅ | ⚠️ | ⚠️ |
| **SharePoint & OneDrive** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Power BI** | ✅ 🏆 | ⚠️ | ⚠️ | ⚠️ | ✅ | ❌ |
| **Dataverse** | ✅ 🏆 | ⚠️ | ⚠️ | ⚠️ | ✅ | ❌ |
| **Salesforce CRM** | ⚠️ | ✅ | ✅ | ✅ | ⚠️ | ✅ |
| **Google Workspace** | ⚠️ | ✅ | ✅ | ✅ | ⚠️ | ✅ |
| **Slack** | ❌ | ✅ | ✅ | ❌ | ⚠️ | ❌ |
| **Data Lakes (Databricks, Delta)** | ⚠️ | ✅ | ✅ | ⚠️ | ⚠️ | ❌ |

**Resumo:** Data Boar é **forte em SQL/NoSQL/APIs**, especialista em **Power BI + Dataverse**.

---

### Cobertura de Dados
| Capacidade | Data Boar | BigID | Collibra | Varonis | Purview | Imperva |
|-----------|---------|-------|---------|---------|---------|---------|
| **Arquivos estruturados (Excel, CSV)** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Documentos não-estruturados (PDF, DOC)** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Imagens (com OCR)** | ✅ | ✅ | ⚠️ | ⚠️ | ⚠️ | ❌ |
| **Arquivos compactados (ZIP, TAR, 7Z)** | ✅ | ✅ | ⚠️ | ✅ | ⚠️ | ❌ |
| **Código-fonte & logs** | ⚠️ | ✅ | ✅ | ⚠️ | ⚠️ | ❌ |
| **Metadados de arquivo** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Detecção por magic bytes** | ✅ 🏆 | ⚠️ | ❌ | ⚠️ | ❌ | ❌ |
| **Cópia zero de PII** | ✅ 🏆 | ✅ | ⚠️ | ✅ | ✅ | ⚠️ |

---

## 2. CLASSIFICAÇÃO & SENSIBILIDADE

### Tipos de Dados
| Tipo | Data Boar | BigID | Collibra | Varonis | Purview | Imperva |
|-----|---------|-------|---------|---------|---------|---------|
| **PII Básico (email, telefone)** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Documentos de ID (CPF, RG, etc)** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Cartão de Crédito / Pagamento** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Dados de Saúde (HIPAA, sensíveis)** | ✅ | ✅ | ✅ | ✅ | ✅ | ⚠️ |
| **Dados Financeiros (salário, conta)** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Dados Biométricos** | ✅ | ✅ | ⚠️ | ✅ | ⚠️ | ⚠️ |
| **Dados de Religião / Opinião Política** | ✅ 🏆 | ⚠️ | ⚠️ | ⚠️ | ⚠️ | ❌ |
| **Dados de Menores (COPPA, LGPD Art 14)** | ✅ 🏆 | ⚠️ | ❌ | ❌ | ❌ | ❌ |
| **Quasi-identificadores (agregação)** | ✅ 🏆 | ⚠️ | ⚠️ | ⚠️ | ⚠️ | ❌ |
| **Dados de Localização (GPS)** | ✅ | ✅ | ⚠️ | ✅ | ⚠️ | ⚠️ |

**Resumo:** Data Boar é **especialista em categorias sensíveis especializadas** (menores, quasi-IDs, religião).

---

### Métodos de Detecção
| Método | Data Boar | BigID | Collibra | Varonis | Purview | Imperva |
|------|---------|-------|---------|---------|---------|---------|
| **Regex customizado** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Keyword matching** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **ML (termos treináveis)** | ✅ | ✅ | ✅ | ✅ | ⚠️ | ✅ |
| **Deep Learning (neural nets)** | ✅ | ✅ | ⚠️ | ⚠️ | ❌ | ⚠️ |
| **Fingerprinting (documento padrão)** | ⚠️ | ✅ | ⚠️ | ⚠️ | ✅ | ⚠️ |
| **Exact Data Match (EDM)** | ⚠️ | ✅ | ⚠️ | ⚠️ | ✅ | ⚠️ |
| **Trainable Classifiers** | ⚠️ | ✅ | ⚠️ | ⚠️ | ✅ | ⚠️ |
| **Statistical analysis (anomalias)** | ⚠️ | ✅ | ⚠️ | ✅ | ⚠️ | ❌ |
| **Context-aware detection** | ✅ 🏆 | ⚠️ | ⚠️ | ⚠️ | ⚠️ | ❌ |

---

## 3. CONFORMIDADE & FRAMEWORKS

### Regulamentações Suportadas
| Regulamento | Data Boar | BigID | Collibra | Varonis | Purview | Imperva |
|-----------|---------|-------|---------|---------|---------|---------|
| **LGPD (Brasil)** | ✅ 🏆 | ⚠️ | ⚠️ | ⚠️ | ⚠️ | ❌ |
| **GDPR (EU)** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **CCPA (Califórnia)** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **UK GDPR** | ✅ | ✅ | ⚠️ | ✅ | ✅ | ⚠️ |
| **HIPAA (Saúde EUA)** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **PCI-DSS (Pagamentos)** | ✅ | ✅ | ⚠️ | ✅ | ✅ | ✅ |
| **GLBA (Financeiro EUA)** | ✅ | ✅ | ⚠️ | ✅ | ✅ | ⚠️ |
| **PIPEDA (Canadá)** | ✅ | ✅ | ⚠️ | ✅ | ⚠️ | ⚠️ |
| **POPIA (África do Sul)** | ✅ | ✅ | ⚠️ | ✅ | ⚠️ | ❌ |
| **APPI (Japão)** | ✅ | ✅ | ⚠️ | ✅ | ⚠️ | ❌ |
| **DPA 152-FZ (Rússia)** | ✅ 🏆 | ⚠️ | ❌ | ⚠️ | ❌ | ❌ |

**Resumo:** Data Boar tem **cobertura mais ampla** de frameworks, especialmente LGPD e regional.

---

## 4. PROTEÇÃO & REMEDIAÇÃO ⚠️ GAPS CRÍTICOS

### Data Masking
| Capacidade | Data Boar | BigID | Collibra | Varonis | Purview | Imperva |
|-----------|---------|-------|---------|---------|---------|---------|
| **Masking em relatório** | ❌ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Masking em dashboard** | ❌ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Masking em DB (dynamic)** | ❌ | ✅ | ⚠️ | ✅ | ✅ | ✅ |
| **Múltiplos tipos (partial, full, hash)** | ❌ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Customizável por padrão** | ❌ | ✅ | ✅ | ✅ | ✅ | ✅ |

### Data Redaction & Removal
| Capacidade | Data Boar | BigID | Collibra | Varonis | Purview | Imperva |
|-----------|---------|-------|---------|---------|---------|---------|
| **Redação em arquivo** | ❌ | ✅ | ✅ | ✅ | ⚠️ | ✅ |
| **Exclusão em banco de dados** | ❌ | ✅ | ⚠️ | ✅ | ⚠️ | ❌ |
| **Purga por retenção** | ❌ | ✅ | ✅ | ✅ | ✅ | ❌ |
| **DSAR automation (direito ao esquecimento)** | ❌ | ✅ | ✅ | ✅ | ✅ | ❌ |

### Remediação Workflow
| Capacidade | Data Boar | BigID | Collibra | Varonis | Purview | Imperva |
|-----------|---------|-------|---------|---------|---------|---------|
| **Approval workflows** | ❌ | ✅ | ✅ | ✅ | ⚠️ | ⚠️ |
| **Automatização end-to-end** | ❌ | ✅ | ✅ | ✅ | ⚠️ | ⚠️ |
| **Rollback/audit trail** | ⚠️ | ✅ | ✅ | ✅ | ✅ | ⚠️ |
| **Playbooks pré-feitos** | ❌ | ✅ | ✅ | ✅ | ⚠️ | ✅ |

---

## 5. MONITORAMENTO & SEGURANÇA

### Activity Monitoring (DAM)
| Capacidade | Data Boar | BigID | Collibra | Varonis | Purview | Imperva |
|-----------|---------|-------|---------|---------|---------|---------|
| **Logging de queries SQL** | ❌ | ✅ | ❌ | ✅ | ✅ | ✅ |
| **Logging de API calls** | ❌ | ✅ | ❌ | ✅ | ⚠️ | ✅ |
| **User entity behavior (UEBA)** | ❌ | ✅ | ❌ | ✅ | ⚠️ | ❌ |
| **Detecção de anomalias** | ❌ | ✅ | ❌ | ✅ | ⚠️ | ⚠️ |
| **Tempo real** | ❌ | ✅ | ❌ | ✅ | ⚠️ | ✅ |

### Alertas & Notificações
| Capacidade | Data Boar | BigID | Collibra | Varonis | Purview | Imperva |
|-----------|---------|-------|---------|---------|---------|---------|
| **Alertas em tempo real** | ❌ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Webhook / integração SIEM** | ⚠️ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Email notifications** | ⚠️ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Slack / Teams integration** | ❌ | ✅ | ✅ | ✅ | ✅ | ⚠️ |
| **Custom escalation** | ❌ | ✅ | ⚠️ | ✅ | ⚠️ | ⚠️ |

---

## 6. GOVERNANÇA & CONTROLE

### Access Control & Governance
| Capacidade | Data Boar | BigID | Collibra | Varonis | Purview | Imperva |
|-----------|---------|-------|---------|---------|---------|---------|
| **Role-based access (RBAC)** | ⚠️ | ✅ | ✅ | ✅ | ✅ | ⚠️ |
| **Atributo-based (ABAC)** | ❌ | ✅ | ✅ | ✅ | ⚠️ | ⚠️ |
| **Column-level access** | ❌ | ✅ | ✅ | ✅ | ✅ | ❌ |
| **Row-level security** | ❌ | ✅ | ✅ | ✅ | ⚠️ | ❌ |
| **Least privilege enforcement** | ❌ | ✅ | ⚠️ | ✅ | ✅ | ⚠️ |
| **Access request workflow** | ❌ | ✅ | ✅ | ✅ | ⚠️ | ❌ |
| **Justification tracking** | ❌ | ✅ | ✅ | ✅ | ⚠️ | ❌ |

### Data Lineage & Genealogy
| Capacidade | Data Boar | BigID | Collibra | Varonis | Purview | Imperva |
|-----------|---------|-------|---------|---------|---------|---------|
| **ETL pipeline tracking** | ❌ | ⚠️ | ✅ | ⚠️ | ⚠️ | ❌ |
| **Data flow visualization** | ❌ | ⚠️ | ✅ | ⚠️ | ⚠️ | ❌ |
| **ROPA mapping (automático)** | ❌ | ⚠️ | ✅ | ⚠️ | ⚠️ | ❌ |
| **Upstream/downstream analysis** | ❌ | ⚠️ | ✅ | ⚠️ | ⚠️ | ❌ |
| **Impact analysis** | ❌ | ⚠️ | ✅ | ⚠️ | ⚠️ | ❌ |

---

## 7. RELATÓRIOS & COMPLIANCE

### Tipos de Saída
| Tipo | Data Boar | BigID | Collibra | Varonis | Purview | Imperva |
|-----|---------|-------|---------|---------|---------|---------|
| **Excel (XLSX)** | ✅ | ✅ | ✅ | ✅ | ⚠️ | ✅ |
| **PDF Report** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **JSON/CSV Export** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Heatmap/Visualização** | ✅ | ✅ | ✅ | ✅ | ⚠️ | ✅ |
| **Executive Summary** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Markdown/RoPA** | ✅ 🏆 | ⚠️ | ✅ | ⚠️ | ⚠️ | ❌ |
| **Customizável** | ✅ | ✅ | ✅ | ✅ | ⚠️ | ✅ |

### Compliance Management
| Capacidade | Data Boar | BigID | Collibra | Varonis | Purview | Imperva |
|-----------|---------|-------|---------|---------|---------|---------|
| **Compliance checklist** | ⚠️ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Control mapping** | ⚠️ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Assessment automation** | ⚠️ | ✅ | ✅ | ✅ | ✅ | ⚠️ |
| **Audit-ready evidence** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Policy versioning** | ⚠️ | ✅ | ✅ | ✅ | ✅ | ⚠️ |

---

## 8. INTEGRAÇÕES & EXTENSIBILIDADE

### Integrações de Segurança
| Integração | Data Boar | BigID | Collibra | Varonis | Purview | Imperva |
|-----------|---------|-------|---------|---------|---------|---------|
| **Splunk** | ⚠️ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Elastic/ELK** | ⚠️ | ✅ | ✅ | ✅ | ⚠️ | ✅ |
| **Sentinel** | ⚠️ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Datadog** | ❌ | ✅ | ⚠️ | ✅ | ⚠️ | ⚠️ |
| **ServiceNow** | ❌ | ✅ | ✅ | ✅ | ⚠️ | ⚠️ |
| **Jira** | ⚠️ | ✅ | ✅ | ⚠️ | ⚠️ | ⚠️ |
| **Webhook genérico** | ⚠️ | ✅ | ✅ | ✅ | ✅ | ✅ |

### Integrações de Dados
| Integração | Data Boar | BigID | Collibra | Varonis | Purview | Imperva |
|-----------|---------|-------|---------|---------|---------|---------|
| **Informatica** | ❌ | ✅ | ✅ | ⚠️ | ⚠️ | ❌ |
| **Talend** | ❌ | ✅ | ✅ | ⚠️ | ⚠️ | ❌ |
| **Alation Catalog** | ❌ | ✅ | ✅ | ⚠️ | ✅ | ❌ |
| **Metadata Exchange** | ⚠️ | ✅ | ✅ | ⚠️ | ✅ | ❌ |
| **API REST** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |

---

## 9. DEPLOYMENT & OPERAÇÕES

### Modelos de Deployment
| Modelo | Data Boar | BigID | Collibra | Varonis | Purview | Imperva |
|------|---------|-------|---------|---------|---------|---------|
| **SaaS** | ❌ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **On-premises** | ✅ | ✅ | ✅ | ✅ | ⚠️ | ✅ |
| **Docker** | ✅ 🏆 | ✅ | ⚠️ | ✅ | ✅ | ✅ |
| **Kubernetes** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Hybrid** | ✅ | ✅ | ✅ | ✅ | ⚠️ | ✅ |
| **Agentless** | ⚠️ | ✅ | ✅ | ⚠️ | ✅ | ✅ |

### Performance & Escalabilidade
| Aspecto | Data Boar | BigID | Collibra | Varonis | Purview | Imperva |
|--------|---------|-------|---------|---------|---------|---------|
| **Suporta TB+ dados** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Amostragem inteligente** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Timeouts configuráveis** | ✅ | ✅ | ⚠️ | ✅ | ⚠️ | ⚠️ |
| **Multi-threaded scanning** | ✅ | ✅ | ⚠️ | ✅ | ⚠️ | ✅ |
| **Scheduled scans** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |

---

## Resumo Final

### 🏆 Forças do Data Boar
1. **Descoberta multi-dialeto** (SQL, NoSQL, APIs, arquivos, Power BI)
2. **Classificação orientada a compliance** (LGPD, GDPR, regional)
3. **Detecção de menores/categorias especializadas**
4. **Configuração YAML** (sem código)
5. **Cópia zero de PII**

### 🔴 Gaps Críticos (MVP)
1. **Data Masking** - ❌ Não possui (Todos têm)
2. **Remediação Automática** - ❌ Não possui (BigID, Varonis)
3. **Access Control** - ❌ Não possui (BigID, Varonis, Purview)
4. **Activity Monitoring** - ❌ Não possui (Varonis, BigID)
5. **Alertas Tempo Real** - ❌ Não possui (Todos têm)

### 💡 Oportunidade Estratégica
- **Posicionamento:** "Descoberta especializada em compliance LGPD"
- **Público-alvo:** Mid-market LGPD/GDPR (Brasil, EU, CA)
- **Preço:** 3-5x menor que BigID/Varonis
- **Timeline MVP:** 90 dias (10 features críticas)

---

**Documento de Referência Rápida**  
Data: 2 de maio de 2026

