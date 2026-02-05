# Bioestatística Aplicada à Saúde usando R

[![Quarto](https://img.shields.io/badge/Made%20with-Quarto-blue.svg)](https://quarto.org/) [![R](https://img.shields.io/badge/Made%20with-R-276DC3.svg)](https://www.r-project.org/) [![License: CC BY-NC-SA 4.0](https://img.shields.io/badge/License-CC%20BY--NC--SA%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/) [![DOI](https://zenodo.org/badge/1144563682.svg)](https://doi.org/10.5281/zenodo.18458781)

> Material didático interativo para o curso de **Bioestatística Aplicada à Saúde utilizando o Software R**

## Sobre o Livro

Este livro foi desenvolvido como material didático para o a disciplina de Bioestatística Aplicada à Saúde utilizando o software R ofertada para o Doutorado em Promoção da Saúde da Universidade Cesumar. Ele combina teoria estatística com aplicações práticas usando R, focando em dados reais da área da saúde.

**Autores:**

-   Prof. Dr. Audrei Pavanello
-   Profa. Dra. Karina Miura da Costa
-   Prof. Dr. Leonardo Pestillo de Oliveira

## Objetivos

-   Ensinar conceitos fundamentais de bioestatística
-   Desenvolver habilidades práticas em R e RStudio
-   Aplicar técnicas estatísticas a dados reais de saúde
-   Criar visualizações informativas e profissionais
-   Interpretar e comunicar resultados de análises

## Conteúdo

### Capítulo 1: Introdução ao R e RStudio

-   O que é o R?
-   Instalação e configuração
-   Interface do RStudio
-   Primeiros passos com R
-   Boas práticas de codificação

### Capítulo 2: Manipulação de Dados com Tidyverse

-   Introdução ao Tidyverse
-   Importação de dados
-   Transformação de dados (select, filter, mutate, etc.)
-   Pipe operator (`|>`)
-   Trabalhando com dados tidy

### Capítulo 3: Análise Exploratória de Dados

-   Visualização de dados com ggplot2
-   Estatística descritiva com gtsummary

### Capítulo 4: Testes Estatísticos

-   Testes de hipóteses paramétricos e não-paramétricos
-   Teste t, ANOVA, Mann-Whitney, Kruskal-Wallis
-   Qui-quadrado, Fisher, e correlações

### Capítulo 5: Regressão Linear, Logística e Multinomial

-   Regressão linear simples e múltipla
-   Regressão logística e multinomial
-   Interpretação de coeficientes e odds ratio

### Capítulo 6: Análise Psicométrica

-   Confiabilidade (Alfa de Cronbach, Ômega de McDonald)
-   Análise Fatorial Exploratória e Confirmatória
-   Validação de escalas e instrumentos

### Capítulo 7: Análise de Dados Textuais e Mineração de Texto

-   Tokenização e análise de frequência
-   Word clouds e análise de sentimento
-   Regressão multinomial para dados textuais

## Como Usar Este Livro

### Pré-requisitos

-   **R** (versão 4.3 ou superior): [Download](https://cran.r-project.org/)
-   **RStudio** (recomendado): [Download](https://posit.co/download/rstudio-desktop/)

### Instalação de Pacotes

Execute o seguinte código no R para instalar todos os pacotes necessários:

``` r
# Lista de pacotes necessários
packages <- c(
  # Básicos (Capítulos 1-3)
  "tidyverse", "readxl", "janitor", "gtsummary",
  "officer", "flextable", "ggpubr",
  # Testes Estatísticos (Capítulo 4)
  "car", "FSA", "vcd",
  # Regressão (Capítulo 5)
  "jtools", "sjPlot", "lmtest", "nnet",
  # Análise Psicométrica (Capítulo 6)
  "psych", "lavaan", "semPlot", "qgraph", "corrplot", "GGally",
  # Análise de Texto (Capítulo 7)
  "tidytext", "wordcloud", "quanteda", "lexiconPT", "tm", "stopwords"
)

# Instalar pacotes que não estão instalados
install.packages(setdiff(packages, rownames(installed.packages())))

# Para mall (análise de texto com LLMs) - opcional
# install.packages("mall")
# Requer instalação prévia do Ollama: https://ollama.com/download
```

### Visualizando o Livro Online

**Acesse o livro publicado em**: \[https://audreipavanello.github.io/bioestatistica_r/\]

## Dados

Os dados utilizados neste livro são de **internações hospitalares em Maringá-PR (2024)**, obtidos do Sistema de Informações Hospitalares do SUS (SIH/SUS).

-   **Arquivo**: `data/dados_internacoes_maringa_2024.xlsx`
-   **Fonte**: DATASUS

## Como Citar Este Livro

Se você utilizar este material em sua pesquisa ou trabalho acadêmico, por favor cite da seguinte forma:

**ABNT:**

```
PAVANELLO, A.; da COSTA, K. M.; OLIVEIRA, L. P. Bioestatística Aplicada à Saúde usando R. Maringá: Universidade Cesumar, 2026. Disponível em: https://audreipavanello.github.io/bioestatistica_r/. DOI: 10.5281/zenodo.18458781.
```

**APA 7th Edition:**

```
Pavanello, A., da Costa, K. M., & Oliveira, L. P. (2026). Bioestatística Aplicada à Saúde usando R. Universidade Cesumar. https://doi.org/10.5281/zenodo.18458781
```

**BibTeX:**

``` bibtex
@book{pavanello2026bioestatistica,
  title={Bioestatística Aplicada à Saúde usando R},
  author={Pavanello, Audrei and Costa, Karina Miura and Oliveira, Leonardo Pestillo},
  year={2026},
  publisher={Universidade Cesumar},
  address={Maringá, PR},
  url={https://audreipavanello.github.io/bioestatistica_r/},
  doi={10.5281/zenodo.18458781}
}
```

## Licença

**Conteúdo**: [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0)](https://creativecommons.org/licenses/by-nc-sa/4.0/)

Você é livre para:

-   **Compartilhar** — copiar e redistribuir o material
-   **Adaptar** — remixar, transformar e criar a partir do material

Sob os seguintes termos:

-   **Atribuição** — Você deve dar crédito apropriado
-   **Não Comercial** — Você não pode usar para fins comerciais
-   **Compartilha Igual** — Distribuições devem usar a mesma licença

**Código**: [MIT License](https://opensource.org/licenses/MIT)

**Última atualização**: Janeiro 2026
