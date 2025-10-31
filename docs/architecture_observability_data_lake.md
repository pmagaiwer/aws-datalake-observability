# Arquitetura de Monitoramento – Data Lake AWS

```mermaid
flowchart LR
    Data-Producers["Sistemas - APIs - Streams - etc"]
    AWS-S3["Storage - Availability"]
    AWS-Glue-ETL["Jobs - Transformações"]
    AWS-EMR["Spark - Hive - Hadoop"]
    AWS-Athena["Queries - Consumo final"]
    Observabilidade-CloudWatch+Grafana["Observabilidade-CloudWatch+Grafana"]
    Prometheus["Prometheus"]
    Alertmanager["Alertmanager"]
    ServiceNow["ServiceNow"]

    Data-Producers --> Observabilidade-CloudWatch+Grafana
    AWS-S3 --> Observabilidade-CloudWatch+Grafana
    AWS-Glue-ETL --> Observabilidade-CloudWatch+Grafana
    AWS-EMR --> Observabilidade-CloudWatch+Grafana
    AWS-Athena --> Observabilidade-CloudWatch+Grafana
    Observabilidade-CloudWatch+Grafana --> Prometheus --> Alertmanager --> ServiceNow
```