# Previsão de Volatilidade de Preços do Diesel S10

Projeto de machine learning sobre dados públicos da ANP para identificar semanas de alta volatilidade no preço do Óleo Diesel S10 nos estados brasileiros.

## O que o projeto faz

O modelo não tenta prever o valor exato do preço do diesel na próxima semana. Em vez disso, ele analisa o comportamento recente dos preços e responde uma pergunta mais simples: **essa semana tem chance de ser uma semana de choque?**

A saída é binária — alerta de alta volatilidade ou não — o que torna o resultado diretamente útil para decisões de estoque, contratos e planejamento logístico.

## Fonte dos dados

[ANP — Série Histórica do Levantamento de Preços de Combustíveis](https://www.gov.br/anp/pt-br/assuntos/precos-e-defesa-da-concorrencia/precos/precos-revenda-e-de-distribuicao-combustiveis/serie-historica-do-levantamento-de-precos)

O arquivo utilizado é o de preços semanais por estado, disponível publicamente no site da agência. Coloque o arquivo baixado em `raw/semanal-estados-desde-2013.xlsx`.

## Estrutura do repositório

```
├── raw/
│   └── semanal-estados-desde-2013.xlsx   # Dados brutos da ANP (não versionado)
├── notebooks/
│   └── 01_data_understanding_v2.ipynb    # Notebook principal
├── relatorio_prospeccao.docx             # Relatório do projeto
└── README.md
```

## Como executar

**1. Clone o repositório**
```bash
git clone <url-do-repositorio>
cd <nome-do-repositorio>
```

**2. Instale as dependências**
```bash
pip install pandas numpy matplotlib seaborn scikit-learn openpyxl
```

**3. Baixe os dados da ANP**

Acesse o link acima, baixe o arquivo de séries históricas semanais por estado e salve em `raw/semanal-estados-desde-2013.xlsx`.

**4. Execute o notebook**
```bash
jupyter notebook notebooks/01_data_understanding_v2.ipynb
```

Execute as células na ordem. Todas as seções são independentes entre si, mas precisam ser rodadas sequencialmente.

## Resumo do pipeline

| Seção | O que faz |
|---|---|
| 1 | Carrega o arquivo Excel da ANP |
| 2 | Limpa os dados: remove colunas com mais de 50% de valores ausentes e linhas sem preço ou data |
| 3 | Filtra para Diesel S10 a partir de 2021 |
| 4 | Análise exploratória: evolução de preços, volatilidade por estado e por região |
| 5 | Cria os indicadores usados pelo modelo e define a variável alvo |
| 6 | Treina o modelo com divisão temporal treino/teste |
| 7 | Avalia os resultados e exibe a importância de cada variável |

## Modelo

**Random Forest** com 200 árvores, profundidade máxima de 10 e mínimo de 5 registros por folha.

A variável alvo é calculada com base no percentil 90 das variações semanais do conjunto de treino — semanas com variação absoluta acima desse limiar são classificadas como alta volatilidade. Com os dados do período 2021–2026, o limiar resultou em aproximadamente 2,6%, com 9,4% dos registros marcados como positivos.

O conjunto de teste nunca é usado para nenhum cálculo durante o treinamento, incluindo a definição do limiar.

## Limitações conhecidas

- O modelo não incorpora variáveis externas como preço do petróleo Brent e câmbio USD/BRL, que têm alto poder explicativo sobre o preço do diesel.
- Choques sem precedente histórico (eventos geopolíticos, mudanças abruptas de política de preços) não são capturados adequadamente.
- A validação foi feita com um único split temporal. Uma validação com múltiplas janelas seria mais robusta.

## Requisitos

- Python 3.10+
- pandas, numpy, matplotlib, seaborn, scikit-learn, openpyxl
