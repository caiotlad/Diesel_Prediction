# Diesel S10 Volatility Forecasting — Brazil

Projeto de análise de dados e machine learning voltado para previsão de volatilidade no preço do Diesel S10 utilizando dados públicos da ANP.

O objetivo do projeto é identificar padrões temporais no comportamento dos preços dos combustíveis e desenvolver um modelo capaz de antecipar semanas de alta volatilidade, auxiliando processos de análise de risco operacional, logística e tomada de decisão.

---

# Business Problem

Oscilações abruptas no preço do Diesel impactam diretamente:

- custos logísticos;
- planejamento operacional;
- distribuição;
- transporte rodoviário;
- previsibilidade financeira.

Neste contexto, prever períodos de maior instabilidade pode ajudar empresas a:

- antecipar riscos;
- ajustar planejamento;
- reduzir exposição a variações bruscas;
- melhorar previsibilidade operacional.

O projeto trata o problema como uma tarefa de classificação temporal:

> A próxima semana apresentará alta volatilidade no preço do Diesel S10?

---

# Dataset

Fonte:

- Agência Nacional do Petróleo, Gás Natural e Biocombustíveis (ANP)

Base utilizada:

- Série histórica semanal por estado
- Combustível: Diesel S10
- Período analisado: 2021–2026

Os dados incluem:

- preço médio de revenda;
- preço mínimo e máximo;
- desvio padrão;
- coeficiente de variação;
- distribuição regional e estadual.

---

# Pipeline

## 1. Data Cleaning

A base original apresentou os problemas:

- cabeçalhos institucionais;
- valores ausentes representados por "-";
- colunas numéricas tratadas como texto;
- inconsistências estruturais.

Todos foram devidamente tratados para tornar possível a análise.

---

## 2. Exploratory Data Analysis (EDA)

Análises realizadas:

- evolução temporal do Diesel S10;
- comparação regional;
- estados com maior volatilidade;
- comportamento histórico da dispersão dos preços.

### Insights

- Estados da região Norte apresentaram maior volatilidade média;
- Tendências de preço possuem persistência temporal;
- Volatilidade recente possui forte relação com instabilidade futura.

---

### 3. Features criadas

| Feature | Descrição |
|---|---|
| `VARIACAO_SEMANAL` | Alteração percentual semanal |
| `MEDIA_MOVEL_4_SEMANAS` | Tendência recente |
| `VOLATILIDADE_4_SEMANAS` | Instabilidade recente |
| `LAG_1_SEMANA` | Variação de semana antes da observação atual |
| `LAG_2_SEMANAS` | Variação de duas semanas antes da observação atual |

---

# Machine Learning

## Objetivo do Modelo

Prever semanas futuras de alta volatilidade no Diesel S10.
