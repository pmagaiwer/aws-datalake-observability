# 🧭 Visão Geral do Dashboard

**Nome:** 🧩 Data Lake Reliability – AWS Glue & EMR  
**Objetivo:** Monitorar a confiabilidade, performance e custo das cargas de dados do Data Lake AWS.

---

## 📍 Painéis principais

- ✅ Visão geral de confiabilidade (SLI/SLO geral)  
- 🧩 AWS Glue – sucesso e duração dos jobs  
- ⚙️ AWS EMR – performance e utilização  
- 🚨 Alertas e erros críticos  

---

## 🧱 1️⃣ Estrutura do Dashboard (visão hierárquica)

📊 Data Lake Reliability Dashboard
├── 1️⃣ Overview
│ ├── SLI: % de jobs Glue concluídos
│ ├── SLI: Tempo médio de execução Glue
│ ├── SLI: Tempo médio dos jobs EMR
│ └── SLO: 99% jobs Glue e EMR completando < 15min
├── 2️⃣ AWS Glue
│ ├── Painel: Taxa de sucesso dos jobs ETL
│ ├── Painel: Duração média dos jobs (P95)
│ ├── Painel: Falhas por job
│ └── Painel: Tempo de fila/execução (start delay)
├── 3️⃣ AWS EMR
│ ├── Painel: Duração média de jobs Spark
│ ├── Painel: Falhas por job e tipo
│ ├── Painel: Uso de CPU e memória por cluster
│ ├── Painel: Tempo médio de provisionamento
│ └── Painel: Custo estimado (EC2 + EMR)
└── 4️⃣ Alertas e Logs
├── Painel: Alertas críticos (Alertmanager)
└── Painel: Últimos logs de falha (CloudWatch Logs Insights)


---

## 📊 2️⃣ Exemplo de Métricas Reais (CloudWatch + Prometheus)

### 🔹 Painel AWS Glue

| Métrica | Descrição | Fonte | SLI | SLO |
|----------|------------|--------|------|------|
| `aws_glue_job_run_succeeded_sum` | Total de execuções bem-sucedidas | CloudWatch Metric | % sucesso = succeeded / total | ≥ 99% |
| `aws_glue_job_run_failed_sum` | Total de execuções com erro | CloudWatch Metric | — | — |
| `aws_glue_job_run_duration_seconds` | Duração média dos jobs ETL | CloudWatch Metric | P95 < 900s | ≤ 15 min |
| `custom_glue_job_queue_delay` | Tempo entre agendamento e início | Exporter custom | < 60s | ≤ 1 min |

➡️ **Consulta PromQL simulada:**

```promql
# SLI 1: Taxa de sucesso dos jobs Glue (7d)
(sum(increase(aws_glue_job_run_succeeded_sum[7d])) 
 / sum(increase(aws_glue_job_run_started_sum[7d]))) * 100

# SLI 2: Duração média dos jobs Glue (15min window)
avg_over_time(aws_glue_job_run_duration_seconds[15m])
```

🔹 Painel AWS EMR

| Métrica                                     | Descrição                         | Fonte                     | SLI        | SLO              |
| ------------------------------------------- | --------------------------------- | ------------------------- | ---------- | ---------------- |
| `aws_emr_job_completed_total`               | Total de jobs Spark concluídos    | Prometheus / JMX exporter | % sucesso  | ≥ 99%            |
| `aws_emr_job_failed_total`                  | Total de falhas                   | Prometheus / JMX exporter | —          | —                |
| `spark_executor_cpu_time_seconds_total`     | Tempo total de CPU dos executores | Prometheus                | —          | Utilização < 80% |
| `aws_emr_cluster_provisioning_time_seconds` | Tempo de inicialização do cluster | CloudWatch Metric         | P95 < 300s | ≤ 5 min          |



➡️ Consulta PromQL simulada:

```promql
# SLI 1: Sucesso dos jobs EMR
(sum(increase(aws_emr_job_completed_total[7d])) 
 / sum(increase(aws_emr_job_started_total[7d]))) * 100

# SLI 2: Latência média (P95)
histogram_quantile(0.95, sum(rate(aws_emr_job_duration_seconds_bucket[5m])) by (le))
```


🧮 3️⃣ Cálculo de SLO no Grafana (exemplo em JSON/YAML)

```yaml
service: "aws-glue-etl"
indicators:
  - name: "job_success_rate"
    metric_query: |
      (sum(increase(aws_glue_job_run_succeeded_sum[30d])) 
       / sum(increase(aws_glue_job_run_started_sum[30d]))) * 100
    target: 99.0
    alert:
      warning: "< 99"
      critical: "< 98"

  - name: "job_duration_p95"
    metric_query: "histogram_quantile(0.95, sum(rate(aws_glue_job_run_duration_seconds_bucket[5m])) by (le))"
    target: 900
    alert:
      warning: "> 900"
      critical: "> 1200"
```


🧠 4️⃣ Visualização no Grafana (layout sugerido)

📊 Data Lake Reliability
───────────────────────────────────────────────
🟢 Glue Success Rate (SLI)
▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇  99.3%

🕐 Glue Job Duration (P95)
▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇  870s

🔥 EMR Job Success Rate
▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇  99.8%

💸 EMR Cost Trend (R$/dia)
💹 line chart showing EC2 + EMR cost

⚠️ Recent Incidents
Glue ETL job “user_data_etl” failed @ 2025-10-31T08:45


🚀 5️⃣ Consolidando SLIs e SLOs

“Eu criaria um dashboard no Grafana consolidando SLIs e SLOs de confiabilidade do Data Lake — por exemplo, a taxa de sucesso dos jobs Glue e EMR, tempo médio de execução e latência P95.
As métricas viriam do CloudWatch e Prometheus, e seriam correlacionadas com custo e capacidade.
Assim conseguimos identificar gargalos antes de impactar as consultas no Athena ou os consumidores de dados.”