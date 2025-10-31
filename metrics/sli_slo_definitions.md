# Definições de SLI e SLO — AWS Data Lake

| Serviço | Tipo | SLI | SLO | Descrição |
|----------|------|-----|-----|------------|
| Glue | Disponibilidade | % de jobs concluídos com sucesso | ≥ 99% | Mede resiliência dos jobs ETL |
| Glue | Performance | Tempo P99 de execução | ≤ 300s | Mede latência de execução |
| EMR | Disponibilidade | % de clusters iniciados com sucesso | ≥ 98% | Mede confiabilidade da orquestração |
| EMR | Performance | Startup time P99 | ≤ 180s | Mede agilidade de provisionamento |
| Athena | Confiabilidade | % de queries sem erro | ≥ 98% | Mede sucesso das consultas |
| S3 | Armazenamento | Disponibilidade do bucket | ≥ 99.9% | Mede acessibilidade dos dados |
