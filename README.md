# ☁️ AWS Data Lake Observability — SLI/SLO Dashboard

Projeto pessoal de observabilidade simulada para um **Data Lake AWS** usando métricas de **SLI/SLO** monitoradas via **Grafana + Prometheus + CloudWatch**.

## 🎯 Objetivo
Demonstrar como implementar um monitoramento de **confiabilidade de dados (Data Reliability Engineering)** para pipelines em ambiente **AWS Data Lake**, aplicando conceitos de **SRE**.

## 🧱 Arquitetura Simulada
Serviços principais monitorados:
- AWS S3 → Armazenamento central de dados brutos e processados
- AWS Glue → ETL jobs (extração e transformação de dados)
- AWS EMR → Processamento distribuído (Spark/Hadoop)
- AWS Athena → Consultas SQL serverless
- AWS Lake Formation → Controle de acesso e governança

Ferramentas de observabilidade:
- Prometheus (coleta de métricas)
- Grafana (visualização e SLI/SLO)
- CloudWatch Exporter (para extrair métricas da AWS)

## 📊 Painéis de Monitoramento
| Painel | Métrica | Descrição |
|--------|----------|------------|
| 🟢 Glue Job Success Rate | glue_job_success_rate | Percentual de jobs ETL com sucesso |
| 🔵 Glue Job Duration P99 | glue_job_duration_p99 | Latência (percentil 99) |
| 🟣 Job Failures per Hour | glue_job_failures | Total de falhas no período |
| 🟠 EMR Bytes Processed | emr_bytes_processed | Volume processado |
| 🟡 EMR Startup Time P99 | emr_cluster_startup_time_p99 | Tempo de inicialização (P99) |
| 🟤 Athena Query Error Rate | athena_query_error_rate | Erros em queries |
| ⚫ SLO Pipeline Availability | — | Cálculo agregado (Glue + EMR + Athena) |
| 🧠 S3 Bucket Size | s3_bucket_size_bytes | Crescimento do Data Lake |

## 📈 O que é P99?
**P99 (percentil 99)** indica que 99% das execuções estão abaixo desse tempo de resposta.
Serve para detectar **outliers e degradações de performance** que não aparecem na média.

## ⚙️ Como usar o dashboard
1. Acesse Grafana → Dashboards → Import
2. Faça upload do JSON em `grafana/aws_datalake_sli_slo_dashboard.json`
3. Configure a fonte de dados Prometheus ou CloudWatch

## 💬 Autor
**Pierre Santos**  
DevOps | SRE | Cloud Engineer
