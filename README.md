# 📊 Análise de Intoxicações Exógenas no Brasil (SINAN / SUS)

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-150458.svg)](https://pandas.pydata.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

Este repositório contém o notebook de análise de dados das **Intoxicações Exógenas do Sistema de Informação de Agravos de Notificação (SINAN/SUS)**, referente ao período de **2006 a 2026**. O projeto abrange desde o download automatizado de microdados até a higienização, padronização, enrich de dados geográficos e exportação para formatos otimizados para BI e Machine Learning.

---

## 👩‍💻 Autoria e Desenvolvimento

* **Autora:** Giedry Fernanda Trindade  
* **Repositório:** [https://github.com/usuario/repositorio](https://github.com/usuario/repositorio)

---

## 🎯 Objetivos do Projeto

* **Coleta e Consolidação:** Processar microdados epidemiológicos estaduais e nacionais do SINAN/DATASUS entre 2006 e 2026.
* **Padronização e Qualidade:** Tratamento rigoroso de *data wrangling* para categorização de códigos.
* **Geração de Métricas e Indicadores:** Preparação de bases limpas para suportar análises de impacto à saúde pública, perfil demográfico das vítimas e substâncias causadoras de intoxicação.
* **Estruturação de Outputs:** Disponibilização de dados tratados e agregados prontos para ingestão em ferramentas de visualização (Power BI, Tableau) e modelos estatísticos.

---

## 🔄 Pipeline de Tratamento de Dados (15 Passos)

O notebook executa um pipeline sequencial estruturado em 15 etapas de processamento e higienização:

```
[DATASUS / SINAN] ──> [1-3. Extração & Carga] ──> [4-7. Mapeamento & Limpeza]
                                                         │
[12-15. Exportação] <── [10-11. Agregações] <── [8-9. Deduplicação & Enriquecimento]
```

1. **Obtenção dos Microdados:** Download automatizado ou leitura dos arquivos brutais do DATASUS (formatos `.dbc` / `.csv`).
2. **Normalização do Esquema de Colunas:** Unificação do *casing* e nomenclatura das colunas para padronização minúscula e sem caracteres especiais.
3. **Mapeamento de Tipos Primários:** Conversão explícita de tipos de dados (datas, inteiros, categóricos e textos).
4. **Decodificação de Categorias Sanitárias:** Tradução dos códigos numéricos do SINAN para rótulos legíveis (ex.: sexo, raça/cor, faixa etária, zona de residência).
5. **Classificação do Agente Tóxico:** Padronização das categorias de substâncias (Medicamentos, Agrotóxicos, Raticidas, Produtos Domésticos, Animais Peçonhentos, etc.).
6. **Padronização da Circunstância da Intoxicação:** Mapeamento do motivo do evento (Tentativa de Suicídio, Acidente Individual, Uso Sanitário, Ocupacional, etc.).
7. **Tratamento de Datas e Lógica Temporal:** Validação das datas de notificação, ocorrência e internação/desfecho, eliminando inconsistências cronológicas.
8. **Enriquecimento Geográfico (IBGE):** Cruzamento dos códigos de municípios de notificação e residência com tabelas de referência do IBGE para inclusão de nomes de cidades, UF e regiões.
9. **Higienização de Textos e Valores Nulos:** Limpeza de strings, tratamento de valores ausentes (`NaN`/`Ignorado`) e preenchimento sistemático.
10. **Tratamento e Classificação da Evolução do Caso:** Categorização do desfecho clínico (Cura, Óbito por intoxicação, Óbito por outra causa, Sequela, etc.).
12. **Criação de Indicadores Agregados:** Geração de tabelas agrupadas com contagens por ano, UF, faixa etária, agente e evolução.
13. **Geração da Tabela Fato Higienizada:** Consolidação dos dados detalhados (*granularidade no nível de notificação*) em formato colunar de alto desempenho.
14. **Geração das Tabelas Dimensão:** Isolamento das entidades categóricas para composição do modelo relacional/Star Schema.
15. **Exportação Multi-formato:** Salvamento das bases processadas nos formatos `.csv` e `.parquet` para máxima compatibilidade e performance.

---

## 🛠️ Tecnologias Utilizadas

* **Linguagem:** Python 3.9+
* **Manipulação de Dados:** `pandas`, `numpy`
* **Leitura de Dados Sanitários:** `pysus` / `dbfread` (para extração DATASUS/DBC)
* **Ambiente de Desenvolvimento:** Jupyter Notebook / Google Colab
* **Formatos de Saída:** Parquet, CSV

---

## 🗄️ Estrutura das Tabelas de Saída

Ao final da execução do pipeline, são gerados os seguintes datasets na pasta de outputs:

| Tabela | Formato | Descrição |
| :--- | :--- | :--- |
| `fato_intoxicacoes` | `.parquet` / `.csv` | Registro detalhado de cada notificação limpa e enriquecida. |
| `dim_agente_toxico` | `.parquet` / `.csv` | Tabela dimensão com a hierarquia dos agentes causadores. |
| `dim_localidade` | `.parquet` / `.csv` | Cadastro de municípios, UFs e regiões com dados IBGE. |
| `agg_intoxicacoes_ano_uf` | `.csv` | Dados agregados prontos para dashboards (Volume de casos e óbitos por ano e estado). |

---

## ⚙️ Requisitos e Instalação

### Pré-requisitos
* Python 3.9 ou superior
* Git

### Passo a Passo

1. **Clonar o repositório:**
   ```bash
   git clone https://github.com/usuario/repositorio.git
   cd repositorio
   ```

2. **Criar e ativar um ambiente virtual (recomendado):**
   ```bash
   pip install uv
   uv venv .venv
   # Linux/macOS:
   source .venv/bin/activate
   # Windows:
   .venv\Scripts\activate
   ```

3. **Instalar as dependências:**
   ```bash
   uv pip install -r requirements.txt
   ```

4. **Executar o Notebook:**
   ```bash
   uv run jupyter notebook notebooks/analise_intoxicacoes_sinan.ipynb
   ```

---

## 📚 Fontes de Dados e Referências

* **DATASUS / SINAN:** [Sistema de Informação de Agravos de Notificação](http://tabnet.datasus.gov.br/) — Ministério da Saúde do Brasil.
* **IBGE:** [Código de Municípios e Divisão Territorial do Brasil](https://www.ibge.gov.br/).
* **Dicionário de Dados SINAN:** Documentação oficial sobre a estrutura de variáveis de Intoxicação Exógena.

---

## 📜 Licença

Este projeto é disponibilizado sob a licença **MIT**. Sinta-se à vontade para utilizar, modificar e distribuir com as devidas atribuições de autoria.
