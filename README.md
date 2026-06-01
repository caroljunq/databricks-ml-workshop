# databricks-ml-workshop
%md
# Visão geral do workshop

## Propósito do workshop

Este workshop demonstra uma jornada completa de machine learning no Databricks para **risco de crédito e decisão de concessão**. O fluxo cobre desde a **carga de dados brutos em tabelas Delta**, passando por **engenharia de features**, **treinamento e registro de modelos no Unity Catalog**, até **deploy em Model Serving** para inferência em tempo real.

Em termos práticos, o repositório mostra como transformar dados de clientes, bureau de crédito, telecom e transações em um pipeline de ML reproduzível, governado e pronto para produção.

## O que cada notebook faz neste repositório

### 1-Criacao-dados

Responsável por:

* localizar os arquivos CSV da pasta `dados`
* criar tabelas Delta no schema configurado
* aplicar um prefixo opcional aos nomes das tabelas
* registrar a tabela de features com chave primária em `cust_id`

Resultado principal: disponibiliza as tabelas base e a tabela de features no schema do workshop.

### 2-Feature-Engineering

Responsável por:

* explorar os dados carregados
* derivar variáveis de cliente, telco e transações
* consolidar as features por `cust_id`
* publicar a tabela de features para uso posterior em treinamento

Resultado principal: construção da tabela `credit_decisioning_features`, que centraliza os atributos usados no modelo de risco.

### 3-Treinamento

Responsável por:

* carregar a tabela de features
* construir o label de inadimplência a partir de `credit_bureau_gold`
* montar e balancear o dataset de treino
* treinar múltiplos modelos
* registrar o melhor modelo no Unity Catalog e preparar inferência em batch

Resultado principal: geração do dataset de treino e registro de um modelo de risco de crédito pronto para serving.

### 4-Model-Serving

Responsável por:

* localizar o modelo registrado no Unity Catalog
* criar ou atualizar um endpoint de Model Serving
* validar o status do endpoint
* enviar exemplos de predição em tempo real

Resultado principal: disponibilização do modelo como endpoint consumível por aplicações e APIs.

### 5-Model-Serving-Model-Inside-Model(opcional)

Responsável por:

* treinar duas abordagens especializadas de score
* empacotar múltiplos modelos em um único PyFunc customizado
* implementar roteamento interno com base no payload da requisição
* expor esse roteamento em um endpoint de serving

Resultado principal: exemplo avançado de serving com estratégia `model-inside-model`, útil para cenários com múltiplas políticas de decisão.

## Tabelas do dataset

As tabelas criadas neste workshop:

| Tabela | Linhas | Papel no workshop | Principais campos |
| --- | ---: | --- | --- |
| `customer_gold` | 100000 | Base principal de clientes com atributos cadastrais, relacionamento e indicadores financeiros. | `cust_id`, `join_date`, `education`, `marital_status`, `income_annual`, `tot_assets`, `customer_score`, `avg_balance` |
| `telco_gold` | 53326 | Complementa o perfil do cliente com comportamento de telefonia e atraso de pagamentos. | `cust_id`, `is_pre_paid`, `number_payment_delays_last12mo`, `pct_increase_annual_number_of_delays_last_3_year`, `phone_bill_amt`, `avg_phone_bill_amt_lst12mo` |
| `fund_trans_gold` | 100000 | Histórico agregado de transações financeiras por cliente em janelas de 12, 6 e 3 meses. | `cust_id`, `sent_txn_cnt_12m`, `sent_txn_amt_12m`, `rcvd_txn_cnt_6m`, `rcvd_txn_amt_3m` |
| `credit_bureau_gold` | 49440 | Histórico de bureau de crédito usado para sinalizar inadimplência e enriquecer análise de risco. | `CUST_ID`, `CREDIT_DAY_OVERDUE`, `AMT_CREDIT_SUM`, `AMT_CREDIT_SUM_DEBT`, `CREDIT_ACTIVE`, `CREDIT_CURRENCY` |
| `credit_decisioning_features` | 100000 | Tabela consolidada de features pronta para modelagem, com uma linha por cliente e chave primária em `cust_id`. | `cust_id`, `education`, `tenure_months`, `product_cnt`, `revenue_tot`, `income_annual`, `is_pre_paid`, `number_payment_delays_last12mo`, `tot_txn_amt_12m`, `ratio_txn_amt_3m_12m` |

## Leitura funcional das tabelas

### `customer_gold`
Camada de perfil do cliente. Reúne dados cadastrais, relacionamento com a instituição, renda, ativos, saldos e indicadores de comportamento financeiro.

### `telco_gold`
Camada comportamental externa. Introduz sinais alternativos de risco a partir de uso de telefonia e atrasos de pagamento.

### `fund_trans_gold`
Camada transacional. Resume volume e valor de transações recebidas e enviadas em diferentes horizontes de tempo, ajudando a capturar intensidade de uso e liquidez.

### `credit_bureau_gold`
Camada de histórico de crédito. Traz exposição, endividamento e atraso, sendo a principal fonte para derivar o alvo de inadimplência no treinamento.

### `ccredit_decisioning_features`
Camada analítica final. Combina sinais de cliente, telco e transações em uma única tabela pronta para treinamento, serving e reuso em novos experimentos.

## Fluxo resumido do workshop

1. `1-Criacao-dados`: carrega os CSVs e publica as tabelas Delta
2. `2-Feature-Engineering`: consolida as features por cliente
3. `3-Treinamento`: cria label, treina modelos e registra o melhor
4. `4-Model-Serving`: publica o modelo em endpoint
5. `5-Model-Serving-Model-Inside-Model(opcional)`: mostra uma arquitetura avançada de serving com roteamento interno
