# Arquitetura de Monitoramento – Data Lake AWS

```mermaid
flowchart LR
    Data-Producers[" Data-Producers: Sistemas - APIs - Streams - etc"]
    AWS-S3["AWS-S3: Storage - Availability"]
    AWS-Glue-ETL["AWS-Glue-ETL: Jobs - Transformações"]
    AWS-EMR["AWS-EMR: Spark - Hive - Hadoop"]
    AWS-Athena["AWS-Athena: Queries - Consumo final"]
    CloudWatch+Grafana["CloudWatch - Grafana"]
    Prometheus["Prometheus"]
    Alertmanager["Alertmanager"]
    ServiceNow["ServiceNow"]

    Data-Producers --> AWS-S3 --> AWS-Glue --> AWS-EMR --> AWS-Athena --> CloudWatch+Grafana
    Data-Producers --> CloudWatch+Grafana
    AWS-S3 --> CloudWatch+Grafana
    AWS-Glue-ETL --> CloudWatch+Grafana
    AWS-EMR --> CloudWatch+Grafana
    AWS-Athena --> CloudWatch+Grafana
    CloudWatch+Grafana --> Prometheus --> Alertmanager --> ServiceNow
```