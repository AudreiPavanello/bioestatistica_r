# Bioestatística Aplicada à Saúde usando R

[![Quarto](https://img.shields.io/badge/Made%20with-Quarto-blue.svg)](https://quarto.org/)
[![R](https://img.shields.io/badge/Made%20with-R-276DC3.svg)](https://www.r-project.org/)
[![License: CC BY-NC-SA 4.0](https://img.shields.io/badge/License-CC%20BY--NC--SA%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/)

> Material didático interativo para o curso de **Bioestatística Aplicada à Saúde utilizando o Software R**

## Sobre o Livro

Este livro foi desenvolvido como material didático para o curso de pós-graduação em Bioestatística Aplicada à Saúde. Ele combina teoria estatística com aplicações práticas usando R, focando em dados reais da área da saúde.

**Autores:**

- Prof. Dr. Audrei Pavanello
- Profa. Dra. Karina Miura Costa
- Prof. Dr. Leonardo Pestillo de Oliveira

## Objetivos

- Ensinar conceitos fundamentais de bioestatística
- Desenvolver habilidades práticas em R e RStudio
- Aplicar técnicas estatísticas a dados reais de saúde
- Criar visualizações informativas e profissionais
- Interpretar e comunicar resultados de análises

## Conteúdo

### Capítulo 1: Introdução ao R e RStudio

- O que é o R?
- Instalação e configuração
- Interface do RStudio
- Primeiros passos com R
- Boas práticas de codificação

### Capítulo 2: Manipulação de Dados com Tidyverse

- Introdução ao Tidyverse
- Importação de dados
- Transformação de dados (select, filter, mutate, etc.)
- Pipe operator (`|>`)
- Trabalhando com dados tidy

### Capítulo 3: Análise Exploratória de Dados

- Visualização de dados com ggplot2
- Estatística descritiva
- Testes estatísticos
  - Teste t de Student
  - ANOVA
  - Correlação
  - Teste qui-quadrado

### Capítulo 4: Regressão Linear e Logística

- Regressão linear simples e múltipla
- Regressão logística
- Interpretação de coeficientes
- Odds ratio
- Diagnóstico de modelos

## Como Usar Este Livro

### Pré-requisitos

- **R** (versão 4.3 ou superior): [Download](https://cran.r-project.org/)
- **RStudio** (recomendado): [Download](https://posit.co/download/rstudio-desktop/)
- **Quarto** (para renderizar o livro): [Download](https://quarto.org/docs/get-started/)

### Instalação de Pacotes

Execute o seguinte código no R para instalar todos os pacotes necessários:

```r
# Lista de pacotes necessários
packages <- c(
  "tidyverse",
  "readxl",
  "janitor",
  "gtsummary",
  "officer",
  "flextable",
  "jtools",
  "car",
  "sjPlot",
  "vcd",
  "lmtest",
  "ggpubr"
)

# Instalar pacotes que não estão instalados
install.packages(setdiff(packages, rownames(installed.packages())))
```

### Visualizando o Livro Online

**Acesse o livro publicado em**: [URL será adicionado após publicação no GitHub Pages]

### Renderizando Localmente

1. Clone este repositório:

```bash
git clone https://github.com/[usuario]/book-bioestatistica-r.git
cd book-bioestatistica-r
```

2. Abra o RStudio e configure o projeto
3. Renderize o livro:

```bash
quarto preview
```

O livro será aberto automaticamente no seu navegador padrão.

## Dados

Os dados utilizados neste livro são de **internações hospitalares em Maringá-PR (2024)**, obtidos do Sistema de Informações Hospitalares do SUS (SIH/SUS).

- **Arquivo**: `data/dados_internacoes_maringa_2024.xlsx`
- **Fonte**: DATASUS
- **Tratamento**: Dados anonimizados conforme LGPD

## 🛠️ Estrutura do Projeto

```
book-bioestatistica-r/
├── .github/
│   └── workflows/
│       └── quarto-publish.yml    # GitHub Actions para publicação
├── data/
│   └── dados_internacoes_maringa_2024.xlsx
├── pdfs/                         # PDFs das aulas originais
├── scripts/                      # Scripts R originais
├── _quarto.yml                   # Configuração do Quarto
├── .gitignore
├── index.qmd                     # Prefácio
├── introducao-r.qmd              # Capítulo 1
├── manipulacao-dados.qmd         # Capítulo 2
├── analise-exploratoria.qmd      # Capítulo 3
├── regressao.qmd                 # Capítulo 4
├── references.qmd                # Referências
├── styles.css                    # Estilos customizados
└── README.md                     # Este arquivo
```

## Como Contribuir

Contribuições são bem-vindas! Se você encontrou um erro, tem sugestões ou quer adicionar conteúdo:

1. Faça um fork do repositório
2. Crie uma branch para sua feature (`git checkout -b feature/MinhaContribuicao`)
3. Commit suas mudanças (`git commit -m 'Adiciona nova feature'`)
4. Push para a branch (`git push origin feature/MinhaContribuicao`)
5. Abra um Pull Request

Ou simplesmente abra uma [Issue](https://github.com/[usuario]/book-bioestatistica-r/issues) descrevendo o problema ou sugestão.

## Licença

**Conteúdo**: [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0)](https://creativecommons.org/licenses/by-nc-sa/4.0/)

Você é livre para:

- **Compartilhar** — copiar e redistribuir o material
- **Adaptar** — remixar, transformar e criar a partir do material

Sob os seguintes termos:

- **Atribuição** — Você deve dar crédito apropriado
- **Não Comercial** — Você não pode usar para fins comerciais
- **Compartilha Igual** — Distribuições devem usar a mesma licença

**Código**: [MIT License](https://opensource.org/licenses/MIT)

**Última atualização**: Janeiro 2026
