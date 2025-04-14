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

Se quiser, posso montar um fluxograma visual com esses blocos para facilitar ainda mais. Deseja isso?