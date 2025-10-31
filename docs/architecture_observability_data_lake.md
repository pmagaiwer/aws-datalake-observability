# Arquitetura de Monitoramento – Data Lake AWS

```mermaid
flowchart LR
    Data-Producers["Sistemas - APIs - Streams - etc"]
    AWS-S3["Storage - Availability"]
    AWS-Glue-ETL["Jobs - Transformações"]
    AWS-EMR["Spark - Hive - Hadoop"]
    AWS-Athena["Queries - Consumo final"]
    Observabilidade-CloudWatch+Grafana["Prometheus - Alertmanager - ServiceNow"]
    Prometheus["Prometheus"]
    Alertmanager["Alertmanager"]
    ServiceNow["ServiceNow"]

    Data-Producers --> AWS-S3 --> AWS-Glue-ETL --> AWS-EMR --> AWS-Athena --> Observabilidade-CloudWatch+Grafana
    Observabilidade-CloudWatch+Grafana --> Prometheus --> Alertmanager --> ServiceNow
```