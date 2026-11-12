# databricks-ml-workshop

Workshop intermediário de **Machine Learning + MLOps** no Databricks, em um único notebook, com caso de **risco de crédito**.

## Propósito

Demonstrar o fluxo atual recomendado:

1. Dados em Delta / Unity Catalog  
2. Feature Engineering in UC (`FeatureEngineeringClient`)  
3. Treino com MLflow 3 + Models in UC  
4. Promoção **Challenger → Champion** com gate de métrica  
5. Model Serving com **features no payload**  
6. (Opcional) Model-inside-model  

## Como usar

1. Abra `notebook_guia_workshop.ipynb`
2. Preencha as variáveis no topo (`CATALOGO`, `SCHEMA`, `PREFIXO`, `CAMINHO_DADOS`)
3. Execute os módulos 1–6 na ordem (fluxo principal)
4. Opcional só no final (ou pré-executado)

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
notebook_guia_workshop.ipynb   # guia único
assets/                        # diagramas
dados/                         # CSVs do caso de crédito
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
