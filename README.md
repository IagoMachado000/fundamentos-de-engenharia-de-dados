# Fundamentos de Engenharia de Dados
Anotações sobre o curso Fundamentos de Engenharia de Dados https://www.datascienceacademy.com.br/course/fundamentos-de-engenharia-de-dados

Engenharia de dados é a área da tecnologia responsável por **projetar, construir, manter e otimizar sistemas que coletam, armazenam, processam e disponibilizam dados** para análise e uso por outras áreas, como ciência de dados, BI (business intelligence) e engenharia de software.

### Em outras palavras:
O engenheiro de dados prepara os dados brutos — muitas vezes desorganizados, incompletos ou espalhados em vários lugares — e transforma isso em algo **organizado, limpo, acessível e eficiente para análise**.

---

### Principais responsabilidades:
- **Coletar dados** de diversas fontes (bancos de dados, APIs, arquivos, etc.)
- **Criar pipelines de dados (ETL/ELT)** — processos que extraem, transformam e carregam dados para um destino final.
- **Modelar dados** para torná-los mais úteis e acessíveis.
- **Garantir a qualidade dos dados** (dados limpos, consistentes e sem duplicidades).
- **Trabalhar com grandes volumes de dados** (Big Data).
- **Construir e gerenciar data lakes e data warehouses**.
- **Automatizar processos de ingestão e transformação de dados**.
- **Implementar políticas de segurança e governança de dados**.

---

### Tecnologias comuns na engenharia de dados:
- **Linguagens**: Python, SQL, Scala.
- **ETL/ELT**: Apache Airflow, dbt, Talend, Dataflow.
- **Big Data**: Apache Spark, Hadoop.
- **Bancos de dados**: PostgreSQL, MySQL, MongoDB, BigQuery, Snowflake, Redshift.
- **Armazenamento em nuvem**: AWS (S3, Glue), GCP (BigQuery, Cloud Storage), Azure.
- **Ferramentas de orquestração e monitoramento**.

---

### Diferença para outras áreas:
| Área                | Foco principal                                           |
|---------------------|----------------------------------------------------------|
| **Engenharia de Dados** | Infraestrutura e organização dos dados                 |
| **Ciência de Dados**     | Análise, predição e criação de modelos com os dados    |
| **BI (Business Intelligence)** | Visualização de dados para tomada de decisão        |
| **Engenharia de Software** | Desenvolvimento de sistemas e aplicativos             |

---

### GUIA DE ESTUDO E APRENDIZAGEM DA DATA SCIENCE ACADEMY 
[E-book](./pdf/49-E-book%20DSA%20_Guia_De_Estudo_Aprendizagem.pdf)

### Bibliografia, Referências e Links Úteis
[Links](./pdf/10-BibliografiaCap01.pdf)

---

### Pipeline de dados

Um **pipeline de dados** é como uma "linha de montagem" que **pega dados brutos de uma ou mais fontes, processa esses dados em etapas definidas e entrega o resultado pronto para ser usado** — geralmente em um banco de dados, data lake, data warehouse ou ferramenta de análise.

---

### Analogia simples:
Imagine que os dados são grãos de café:

1. **Extração**: pegar os grãos da plantação (dados brutos).
2. **Transformação**: torrar, moer, filtrar (limpar, organizar, juntar).
3. **Carga**: servir o café na xícara (enviar os dados para o destino final).

Esse processo contínuo de **extração, transformação e carga** é conhecido como **ETL (Extract, Transform, Load)** ou **ELT** (quando a transformação vem depois da carga).

---

### Fases de um pipeline de dados:

1. **Extração (Extract)**  
   Captura dados de fontes diversas:
   - APIs
   - Bancos de dados
   - Arquivos CSV, Excel
   - Serviços de terceiros (como Google Analytics, Facebook Ads)

2. **Transformação (Transform)**  
   Prepara os dados:
   - Limpeza (remover duplicatas, preencher valores ausentes)
   - Conversão de formatos
   - Junção de diferentes tabelas
   - Aplicação de regras de negócio

3. **Carga (Load)**  
   Envia os dados para o destino:
   - Banco relacional
   - Data warehouse (BigQuery, Redshift, Snowflake)
   - Data lake

---

### Exemplos de ferramentas de pipeline:

- **Apache Airflow** (orquestração de tarefas e agendamentos)
- **dbt** (transformações SQL em data warehouse)
- **Luigi**, **Prefect**, **Kedro** (orquestração)
- **Talend**, **Informatica**, **AWS Glue**, **GCP Dataflow**

---

### Por que usar um pipeline?

- Automatiza o processo de movimentação e tratamento de dados.
- Garante qualidade e consistência.
- Torna o fluxo de dados escalável e monitorável.
- Facilita a atualização de dados em tempo real ou agendada.

---

### Exemplo de um pipeline de dados

#### 🧠 Cenário:
Temos um arquivo `usuarios.csv` com dados de usuários. Vamos:

1. **Extrair** os dados do CSV.  
2. **Transformar**: remover linhas com e-mails inválidos.  
3. **Carregar**: salvar os dados limpos em um banco SQLite.

---

#### 📁 Exemplo do CSV (`usuarios.csv`):

```csv
id,nome,email
1,Ana,ana@email.com
2,João,joao@email.com
3,Lucas,lucas@email
4,Marina,marina@email.com
```

> Note que o email do Lucas está inválido (sem ".com").

---

#### 💻 Código do pipeline (`pipeline_etl.py`):

```python
import pandas as pd
import sqlite3
import re

# EXTRAÇÃO
def extrair_dados(caminho_csv):
    return pd.read_csv(caminho_csv)

# TRANSFORMAÇÃO
def limpar_dados(df):
    # Remove e-mails inválidos com regex simples
    regex_email = r"[^@]+@[^@]+\.[^@]+"
    df_filtrado = df[df['email'].apply(lambda x: re.match(regex_email, x) is not None)]
    return df_filtrado

# CARGA
def carregar_dados(df, nome_banco):
    conn = sqlite3.connect(nome_banco)
    df.to_sql('usuarios', conn, if_exists='replace', index=False)
    conn.close()

# EXECUÇÃO DO PIPELINE
def pipeline():
    print("Iniciando pipeline...")
    dados = extrair_dados('usuarios.csv')
    print("Extração concluída!")

    dados_limpos = limpar_dados(dados)
    print("Transformação concluída!")

    carregar_dados(dados_limpos, 'dados.db')
    print("Carga concluída! Dados salvos no banco 'dados.db'.")

if __name__ == "__main__":
    pipeline()
```

---

#### 📦 Resultado:
- O pipeline cria um banco SQLite (`dados.db`) com uma tabela `usuarios` contendo **somente os registros válidos**.
- O Lucas será excluído por ter um e-mail inválido.

---

### Componentes de um pipeline de dados

#### ✅ **1. Origem (Source)**  
São os **locais onde os dados se encontram antes de serem processados**.

🔹 **Componentes**:
- Bancos de dados relacionais (MySQL, PostgreSQL, SQL Server)
- Bancos NoSQL (MongoDB, Firebase, Cassandra)
- APIs (Google Ads, Facebook, Stripe, etc.)
- Arquivos (CSV, Excel, JSON, XML)
- Sistemas legados (ERP, CRM, etc.)
- Dados em tempo real (Kafka, IoT)

---

#### ⚙️ **2. Processamento (Processing)**  
É o **coração do pipeline**, onde os dados são extraídos, tratados, organizados e preparados para o uso.

🔹 **Componentes**:

##### A) **Extração (Extract)**  
- Captura os dados da origem.

##### B) **Transformação (Transform)**  
- Limpeza, normalização, enriquecimento e padronização dos dados.

##### C) **Orquestração e Agendamento**
- Coordena a ordem e o momento em que cada tarefa roda.
- Ferramentas: Apache Airflow, Prefect, Dagster

##### D) **Validação e Qualidade de Dados**
- Verifica se os dados estão consistentes e corretos.
- Regras de negócio, alertas e checagens.

##### E) **Monitoramento e Logs**
- Acompanha o status das execuções e detecta falhas.

---

#### 📦 **3. Destino (Target)**  
É onde os dados **processados são armazenados e ficam prontos para uso** por sistemas, BI, machine learning, etc.

🔹 **Componentes**:
- Data Warehouses (BigQuery, Redshift, Snowflake)
- Data Lakes (AWS S3, Azure Data Lake)
- Bancos SQL (PostgreSQL, MySQL)
- NoSQL (MongoDB, Elasticsearch)
- Dashboards de BI (Power BI, Looker, Tableau)
- Modelos de Machine Learning

---

#### 🧠 Visual Resumido:

```plaintext
[ ORIGEM ]
    ↓
[ EXTRAÇÃO ]
    ↓
[ TRANSFORMAÇÃO ]
    ↓
[ CARGA ]
    ↓
[ DESTINO ]
    ↓
[ CONSUMO (dashboards, análises, ML, etc.) ]
```

---

### Pipeline de dados x pipeline ETL

Pipeline de dados e pipeline ETL **nem sempre são a mesma coisa**, mas muitas vezes são usados como **sinônimos** — especialmente em contextos mais simples.

Vamos ver a diferença com clareza:

---

#### ✅ **Pipeline ETL** (Extract, Transform, Load)

É um tipo específico de pipeline de dados que segue **três etapas principais**:

1. **Extract (Extração)** – pega dados brutos de uma ou mais fontes.  
2. **Transform (Transformação)** – limpa, trata e padroniza os dados.  
3. **Load (Carga)** – envia os dados para um destino (ex: data warehouse).

🔹 Muito usado quando o foco está em **mover e preparar dados para análises** ou BI.

---

#### 🔄 **Pipeline de Dados** (Data Pipeline)

É um termo **mais genérico e abrangente**.

Pode incluir:

- Pipelines ETL (ou ELT)
- Pipelines de streaming (dados em tempo real)
- Pipelines de machine learning (com ingestão, treino de modelo, deploy)
- Pipelines de replicação de dados
- Pipelines de integração contínua com dados

Ou seja, **todo pipeline ETL é um pipeline de dados**, mas **nem todo pipeline de dados é ETL**.

---

#### 💡 Exemplo de diferença:

- **Pipeline ETL**: Extrai dados do MySQL, transforma em pandas, carrega no BigQuery.
- **Pipeline de streaming**: Usa Kafka + Spark para processar dados de sensores em tempo real.
- **Pipeline de ML**: Coleta dados, transforma, treina modelo, gera previsões automaticamente.

---

#### 🧠 Resumindo:
| Termo              | Abrangência | Finalidade principal           |
|--------------------|-------------|--------------------------------|
| **Pipeline ETL**   | Mais específico | Movimentação + tratamento de dados |
| **Pipeline de Dados** | Mais amplo     | Qualquer fluxo automatizado de dados |

---

### Principais ferramentas para construir pipeline de dados

#### 🔁 1. Transformação de Dados

#### O que é:
São ferramentas que **tratam, limpam, enriquecem, organizam e preparam os dados** para uso — geralmente após a extração e antes do carregamento final.

#### Tarefas comuns:
- Padronizar nomes de colunas
- Corrigir valores inconsistentes
- Juntar dados de diferentes fontes
- Agregar métricas (ex: soma de vendas por dia)

#### Ferramentas populares:
| Ferramenta     | Descrição breve |
|----------------|------------------|
| **dbt (Data Build Tool)** | Transforma dados usando SQL diretamente no data warehouse. Ideal para times de analytics. |
| **Apache Spark** | Processa grandes volumes de dados em cluster (paralelo), com suporte a batch e streaming. |
| **Pandas (Python)** | Biblioteca poderosa para transformar dados tabulares em notebooks/scripts. Ótima para prototipação. |
| **Apache Beam** | Framework de transformação com suporte a batch e streaming. Roda em Dataflow (GCP), Spark, Flink, etc. |
| **Airbyte / Fivetran / Talend** | Algumas dessas ferramentas também permitem transformações, além da extração/carga. |

---

#### ☁️ 2. Armazenamento e Cloud Computing

#### O que é:
São os **locais onde os dados são armazenados**, organizados e disponibilizados — muitas vezes em nuvem. Também inclui serviços que **escalam automaticamente**, como clusters, servidores e bancos gerenciados.

#### Subdivisões:
- **Data warehouses** (análises)
- **Data lakes** (armazenamento bruto)
- **Bancos relacionais/NoSQL**
- **Infraestrutura em nuvem (IaaS/PaaS)**

#### Ferramentas populares:
| Ferramenta     | Descrição breve |
|----------------|------------------|
| **Google BigQuery** | Data warehouse serverless da GCP, ideal para grandes volumes e consultas rápidas. |
| **Amazon Redshift** | Data warehouse da AWS, otimizado para análises massivas. |
| **Snowflake** | Data warehouse multi-cloud, altamente escalável. |
| **AWS S3** | Armazena arquivos em nuvem (data lake). |
| **Azure Data Lake** | Equivalente ao S3 na Azure. |
| **Databricks** | Plataforma para engenharia e ciência de dados baseada em Spark. |
| **Google Cloud Platform / AWS / Azure** | Provedores cloud com serviços integrados de dados, computação, segurança, etc. |

---

#### ⚡ 3. Real-Time Analytics (Análise em Tempo Real)

#### O que é:
São ferramentas e plataformas voltadas para **ingestão, processamento e análise de dados em tempo real ou quase tempo real**. Ideal para sistemas que precisam de respostas imediatas (ex: detecção de fraudes, monitoramento de sensores, logs).

#### Ferramentas populares:
| Ferramenta     | Descrição breve |
|----------------|------------------|
| **Apache Kafka** | Sistema de mensageria distribuído, ideal para capturar e transmitir eventos em tempo real. |
| **Apache Flink** | Processamento de dados em streaming com baixa latência. |
| **Apache Spark Structured Streaming** | Módulo do Spark para trabalhar com dados em tempo real. |
| **Google Dataflow** | Serviço de stream/batch processing na GCP, baseado em Apache Beam. |
| **Kinesis (AWS)** | Equivalente ao Kafka na AWS, ideal para ingestão de dados em tempo real. |
| **ClickHouse** | Banco OLAP de alta performance, muito usado para analytics em tempo real. |

---

#### 💡 Resumo visual da classificação:

```plaintext
┌────────────────────────────┐
│  Transformação de Dados    │  ← limpeza, preparo, junções
├────────────────────────────┤
│  dbt, Spark, Pandas, Beam  │
└────────────────────────────┘

┌──────────────────────────────┐
│  Armazenamento e Cloud       │  ← onde os dados são guardados
├──────────────────────────────┤
│  BigQuery, Redshift, S3,     │
│  Snowflake, Databricks       │
└──────────────────────────────┘

┌────────────────────────────┐
│  Real-Time Analytics       │  ← dados em tempo real
├────────────────────────────┤
│  Kafka, Flink, Dataflow,   │
│  Spark Streaming, Kinesis  │
└────────────────────────────┘
```

---

