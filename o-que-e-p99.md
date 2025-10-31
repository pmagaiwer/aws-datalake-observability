🧠 O que é P99 (Percentil 99)
```bash
👉 P99 (ou Percentil 99) significa que 99% das medições estão abaixo de um determinado valor — ou seja, apenas 1% dos casos são piores que esse limite.
```

É usado para medir performance percebida pelo usuário real, ignorando exceções isoladas.

Percentil	Significado	Exemplo (latência em ms)
```bash
P50	Mediana – metade das execuções é mais rápida, metade mais lenta	150 ms
P95	95% das execuções são mais rápidas que esse valor	400 ms
P99	99% das execuções são mais rápidas que esse valor (1% mais lentas)	900 ms
```

## 💡 Em Data Reliability, usamos P99 para medir:

Tempo máximo aceitável de execução de jobs Glue

Latência de leitura no S3

Tempo de inicialização de clusters EMR

📌 Assim você evita projetar SLOs só na média (que pode enganar) e garante confiabilidade real.
```bash
☁️ Arquitetura de Monitoramento – Data Lake AWS
                   ┌────────────────────────────────┐
                   │         Data Producers          │
                   │ (Sistemas, APIs, Streams, etc.) │
                   └──────────────┬──────────────────┘
                                  │
                                  ▼
             ┌────────────────────────────┐
             │        AWS S3 (Raw)        │
             │  Storage / Availability    │
             └────────────┬───────────────┘
                          │
                          ▼
             ┌────────────────────────────┐
             │        AWS Glue ETL        │
             │  Jobs / Transformações     │
             └────────────┬───────────────┘
                          │
                          ▼
             ┌────────────────────────────┐
             │     AWS EMR (Processing)   │
             │  Spark / Hive / Hadoop     │
             └────────────┬───────────────┘
                          │
                          ▼
             ┌────────────────────────────┐
             │   AWS Athena (Consulta)    │
             │  Queries / Consumo final   │
             └────────────┬───────────────┘
                          │
                          ▼
         ┌─────────────────────────────────────────┐
         │   Observabilidade: CloudWatch + Grafana │
         │ Prometheus / Alertmanager / ServiceNow  │
         └─────────────────────────────────────────┘
```

## 📊 Dashboards Visuais (simulados) no Grafana
# 🔹 1. Visão Geral – Data Lake Reliability
```bash
─────────────────────────────────────────────
📊 DATA LAKE RELIABILITY – AWS (Last 7d)
─────────────────────────────────────────────
🟢 S3 Availability: 99.98%      (SLO: ≥99.9%)
🟢 Glue Job Success Rate: 99.3% (SLO: ≥99%)
🟢 EMR Job Duration P99: 12m    (SLO: ≤15m)
🟡 Athena Query Latency P99: 2.9s (SLO: ≤3s)
─────────────────────────────────────────────
🕐 Incidents: 1 alert (ETL user_data falhou)
─────────────────────────────────────────────
```

# 👉 Interpretação:
A confiabilidade geral está dentro do esperado, mas o painel sinaliza o SLO de Athena próximo ao limite de 3 segundos — um ponto de atenção para otimização de queries.

# 🔹 2. Painel AWS Glue – ETL Success & Duration
```bash
📈 Glue Job Success Rate (Últimas 24h)
███████████████████████████████████▇▆▆▇▇▇▆▆▆
99.3% ✅
```

```bash
📉 Glue Job Duration (P99)
───────────────────────────────
Hora     | Duração P99 (segundos)
08:00    | 780
10:00    | 820
12:00    | 910 ⚠️
14:00    | 870
───────────────────────────────
SLO: ≤900s
```


# 👉 Interpretação:
Às 12h houve pico de duração acima do P99, indicando lentidão pontual — talvez sobrecarga no EMR ou contenção de recursos.

# 🔹 3. Painel AWS EMR – Cluster Performance
```bash
🔥 EMR Cluster – Job Duration P99 (Últimas 24h)
────────────────────────────────────────────
0h  | ████▆▆▆▆▇▇▇▇▆▇▆▇▇▇▇▇▇▇ 12 min
4h  | ███▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇ 13 min
8h  | ████▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇ 14.5 min ⚠️
────────────────────────────────────────────
CPU Utilization: 75%
Memory Usage: 68%
SLO: ≤15 min
```

# 👉 Interpretação:
O P99 se mantém dentro do limite (SLO ≤ 15 min), mas próximo do topo — possível gargalo em I/O ou tuning Spark.

```bash
🔹 4. Painel Athena – Query Latency e Erros
💡 Athena Query Latency (P99)
```

# O que é P99 (Percentil 99)

👉 P99 (ou Percentil 99) significa que 99% das medições estão abaixo de um determinado valor — ou seja, apenas 1% dos casos são piores que esse limite.

É usado para medir a performance percebida pelo usuário real, ignorando exceções isoladas.

## Tabela de percentis
```bash
| Percentil | Significado | Exemplo (latência em ms) |
|---|---|---:|
| P50 | Mediana – metade das execuções é mais rápida, metade mais lenta | 150 ms |
| P95 | 95% das execuções são mais rápidas que esse valor | 400 ms |
| P99 | 99% das execuções são mais rápidas que esse valor (1% mais lentas) | 900 ms |
```

## Por que usar P99

Em Data Reliability, usamos P99 para medir:

- Tempo máximo aceitável de execução de jobs Glue
- Latência de leitura no S3
- Tempo de inicialização de clusters EMR

Assim você evita projetar SLOs somente na média (que pode enganar) e garante confiabilidade real.

## Arquitetura de Monitoramento – Data Lake AWS

```text
                   ┌─────────────────────────────────┐
                   │         Data Producers          │
                   │ (Sistemas, APIs, Streams, etc.) │
                   └──────────────┬──────────────────┘
                                  │
                                  ▼
             ┌────────────────────────────┐
             │        AWS S3 (Raw)        │
             │  Storage / Availability    │
             └────────────┬───────────────┘
                          │
                          ▼
             ┌────────────────────────────┐
             │        AWS Glue ETL        │
             │  Jobs / Transformações     │
             └────────────┬───────────────┘
                          │
                          ▼
             ┌────────────────────────────┐
             │     AWS EMR (Processing)   │
             │  Spark / Hive / Hadoop     │
             └────────────┬───────────────┘
                          │
                          ▼
             ┌────────────────────────────┐
             │   AWS Athena (Consulta)    │
             │  Queries / Consumo final   │
             └────────────┬───────────────┘
                          │
                          ▼
         ┌─────────────────────────────────────────┐
         │   Observabilidade: CloudWatch + Grafana │
         │ Prometheus / Alertmanager / ServiceNow  │
         └─────────────────────────────────────────┘
```

## Dashboards Visuais (simulados) no Grafana

### 1. Visão Geral – Data Lake Reliability (Last 7d)

- **S3 Availability:** 99.98%  (SLO: ≥99.9%)
- **Glue Job Success Rate:** 99.3% (SLO: ≥99%)
- **EMR Job Duration P99:** 12m (SLO: ≤15m)
- **Athena Query Latency P99:** 2.9s (SLO: ≤3s)

- **Incidentes:** 1 alert (ETL user_data falhou)

**Interpretação:**

A confiabilidade geral está dentro do esperado, mas o painel sinaliza o SLO de Athena próximo ao limite de 3 segundos — um ponto de atenção para otimização de queries.

---

### 2. Painel AWS Glue – ETL Success & Duration

**Glue Job Success Rate (Últimas 24h):** 99.3% ✅

**Glue Job Duration (P99)**

| Hora | Duração P99 (segundos) |
|---|---:|
| 08:00 | 780 |
| 10:00 | 820 |
| 12:00 | 910 ⚠️ |
| 14:00 | 870 |

SLO: ≤900s

**Interpretação:**

Às 12h houve pico de duração acima do P99, indicando lentidão pontual — possivelmente sobrecarga no EMR ou contenção de recursos.

---

### 3. Painel AWS EMR – Cluster Performance

**EMR Cluster – Job Duration P99 (Últimas 24h)**

```text
0h  | ████▆▆▆▆▇▇▇▇▆▇▆▇▇▇▇▇▇▇ 12 min
4h  | ███▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇ 13 min
8h  | ████▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇ 14.5 min ⚠️
```

- CPU Utilization: 75%
- Memory Usage: 68%
- SLO: ≤15 min

**Interpretação:**

O P99 se mantém dentro do limite (SLO ≤ 15 min), mas próximo do topo — possível gargalo em I/O ou necessidade de tuning do Spark.

---

### 4. Painel Athena – Query Latency e Erros

**Athena Query Latency (P99)**

| Data | P99 (segundos) |
|---|---:|
| 29/10 | 2.5 |
| 30/10 | 2.9 ⚠️ |
| 31/10 | 2.1 |

SLO: ≤3.0s
Error Rate: 0.3%

**Interpretação:**

Leve aumento de latência (P99) indica possível backlog de queries ou dados não particionados corretamente.

---

### 5. Painel de Alertas (ServiceNow integrado)

- **[HIGH]** Glue job ETL_user_data falhou 3x seguidas
- **[AUTO-RESOLVED]** EMR Cluster startup delay > 5min

Next Steps:

- Verificar logs Glue (CloudWatch)
- Validar IAM no Lake Formation

---

## Como explicar em uma entrevista

> “No meu modelo de observabilidade para Data Lake, eu estabeleceria SLIs e SLOs específicos para cada componente — como taxa de sucesso dos jobs Glue e latência P99 de jobs EMR.
>
> Essas métricas viriam do CloudWatch e Prometheus e seriam visualizadas no Grafana, com alertas automáticos integrados ao ServiceNow.
>
> O uso do P99 é essencial para entender a experiência real dos consumidores de dados e detectar outliers de performance antes que impactem o negócio.”
