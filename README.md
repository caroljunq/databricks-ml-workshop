# databricks-ml-workshop

Workshop de **Machine Learning na Databricks** com MLOps, em um único notebook, com caso de **risco de crédito**.

## Propósito

Demonstrar o fluxo atual recomendado:

1. Dados em Delta / Unity Catalog  
2. Feature Engineering in UC (`FeatureEngineeringClient`)  
3. Treino com MLflow 3 + Models in UC  
4. Promoção **Challenger → Champion** com gate de métrica  
5. Model Serving com **features no payload**  
6. (Opcional) Model-inside-model  

O notebook também traz, como referência e sem execução, o diagrama de **feature lookup automático** (endpoint recebe só `cust_id` e busca as features no Online Feature Store).

## Como usar

1. Crie o schema de destino no Unity Catalog antes de rodar (o notebook não cria o schema).
2. Abra `notebook_guia_workshop.ipynb`.
3. Execute a primeira célula de código (`%pip install` + `restartPython`).
4. Preencha as variáveis na célula de configuração (`CATALOGO`, `SCHEMA`, `PREFIXO`, `CAMINHO_DADOS`) e execute-a. O restart do Python apaga variáveis, então rode a configuração depois da instalação.
5. Execute os módulos 1–6 na ordem (fluxo principal).
6. O opcional fica no final e só deve ser executado se sobrar tempo.

> Se a sessão expirar durante a provisão do endpoint, rode de novo a configuração e o Módulo 1 antes de invocar o endpoint.

### Variáveis sugeridas (FEVM Carolina)

```python
CATALOGO = "workspace_carolina_ferreira_catalog"
SCHEMA = "ml_workshop"
PREFIXO = "ws_"
CAMINHO_DADOS = "/Volumes/workspace_carolina_ferreira_catalog/ml_workshop/dados"

RUN_SERVING = True
RUN_OPTIONAL = False          # model-inside-model
```

## Agenda 1h30

| Tempo | Bloco |
|---|---|
| 0:00–0:10 | Contexto, modelo de dados e arquitetura |
| 0:10–0:25 | Módulos 1–2 (dados + features) — ideal pré-executado |
| 0:25–0:45 | Módulo 3 (treino + Challenger) |
| 0:45–1:00 | Módulo 4 (promoção Champion) |
| 1:00–1:20 | Módulo 5 (Model Serving) — endpoint pré-criado |
| 1:20–1:30 | Módulo 6 + Q&A |

## Estrutura do repositório

```text
notebook_guia_workshop.ipynb   # guia único (todos os módulos e o opcional)
assets/                        # diagramas (arquitetura do workshop e feature lookup automático)
dados/                         # CSVs do caso de crédito (credit_bureau_gold, customer_gold, fund_trans_gold, telco_gold e credit_decisioning_features)
README.md
```

## Stack

| Camada | Tecnologia |
|---|---|
| Dados | Delta + Unity Catalog |
| Features | Feature Engineering in UC |
| Tracking / registry | MLflow 3 + Models in UC |
| Serving | Mosaic AI Model Serving + features no payload |
| MLOps no workshop | aliases Challenger/Champion + gate de AUC |
| Referência (não executada) | Online Feature Store + lookup automático por `cust_id` |
