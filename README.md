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

### O processo de engenharia de dados

* A engenharia de dados fornece o suporte necessário para que o processo de Ciência de Dados possa ser executado
* A Engenharia de Dados não participa de todo o processo de Ciência de Dados, mas sim das atividades onde deve haver gestão de dados
* Uma das primeiras etapas do processo é a extração de dados de uma ou mais fonts, de dados em batch ou dados em streaming
* A etapa seguinte envolve a preparação dos dados com atividades de limpeza, transformação, enriquecimento e segurança de acesso
* O processo continua com o armazenamento dos dados no destino ou uso em tempo real 
* Mas o que seria o uso dos dados? Análise, gráficos, dashboards, machine learning, IA ou qualquer outra tarefa de Ciência de dados
* O pipeline de dados pode ser executado uma única vez ou diversas vezes
* A execução do pipeline requer monitoramento, segurança, validação e documentação
* O pipeline de dados é então criado, documentado e automatizado
* E então outro processo será iniciado para outro produto de dados da empresa
* Um Arquiteto de Dados é o responsável pro projetar a solução
* O trabalho de um Engenheiro de Dados envolve construir e mantes a solução de dados

---

### Ciclo de vida da engenharia de dados

![Diagrama ciclo de vida da engenharia de dados](./img/ciclo-de-vida-engenhraria-de-dados.png)

#### Fonte de dados (batch e streaming)

As **fontes de dados** podem ser classificadas com base em **como os dados são coletados e processados**, e os dois principais modos são:

#### 1. **Batch (Lote)**

- **O que é**: Os dados são coletados, armazenados por um tempo e processados **em blocos (lotes)** em intervalos definidos (ex: a cada hora, dia, semana).
- **Quando usar**: Quando os dados não precisam ser atualizados em tempo real e podem ser processados periodicamente.
- **Exemplos**:
  - Relatórios financeiros gerados toda madrugada.
  - Importação de dados de um CRM (como Salesforce) uma vez por dia.
  - ETL noturno que processa arquivos CSV enviados por e-mail.
- **Ferramentas comuns**: Apache Airflow, AWS Glue, Talend, dbt, Spark (modo batch).

##### ✅ Vantagens:
- Mais simples de implementar e manter.
- Maior performance para grandes volumes.

##### ❌ Desvantagens:
- Dados não estão atualizados em tempo real.
- Pode haver latência alta (minutos a horas).

#### 2. **Streaming (Tempo real)**

- **O que é**: Os dados são processados **em tempo real ou quase em tempo real**, assim que são gerados.
- **Quando usar**: Quando é necessário agir rapidamente com base nos dados mais recentes.
- **Exemplos**:
  - Detecção de fraudes bancárias com base em transações em tempo real.
  - Leituras de sensores IoT transmitidas continuamente.
  - Cliques em anúncios ou navegação de usuários em um site.
- **Ferramentas comuns**: Apache Kafka, Apache Flink, Spark Streaming, AWS Kinesis, Google Pub/Sub.

##### ✅ Vantagens:
- Reações rápidas a eventos.
- Ideal para sistemas críticos ou interativos.

##### ❌ Desvantagens:
- Mais complexo de desenvolver e monitorar.
- Pode ter maior custo operacional.

#### 3. **Lambda Architecture (Híbrido: Batch + Streaming)**

- **O que é**: Combina o melhor dos dois mundos: uma camada de batch para consistência e uma de streaming para baixa latência.
- **Exemplo**:
  - Dados de pedidos de e-commerce processados em tempo real para atualização de estoque, mas reprocessados à noite em batch para gerar relatórios consolidados.

#### Comparativo rápido:

| Característica     | Batch                          | Streaming                       |
|--------------------|--------------------------------|----------------------------------|
| Latência            | Alta (minutos a horas)         | Baixa (milissegundos a segundos)|
| Volume              | Muito alto de uma vez          | Contínuo e constante             |
| Complexidade        | Menor                          | Maior                            |
| Casos ideais        | BI, relatórios, ETL            | Alertas, logs, sistemas online   |

---

#### Ingestão de dados

**Ingestão de dados** é o processo de **coletar dados de uma ou mais fontes** e **trazê-los para dentro de um sistema de armazenamento ou processamento**, como um data lake, data warehouse, banco de dados ou pipeline.

##### 🔍 Explicando de forma simples:

Imagine que as fontes de dados são como **torneiras espalhadas pela empresa** (sites, bancos de dados, APIs, arquivos, sensores, etc.).  
A ingestão de dados é o **encanamento que conecta essas torneiras ao seu reservatório de dados**, garantindo que a água (dados) chegue limpa, no horário certo e no volume certo.

##### Tipos de Ingestão de Dados:

1. **Batch (lote)**  
   - Dados são coletados em grandes blocos, de tempos em tempos.  
   - Ex: Buscar um arquivo CSV por FTP toda noite às 2h da manhã.

2. **Streaming (tempo real)**  
   - Dados são ingeridos continuamente, conforme são gerados.  
   - Ex: Processar eventos do Kafka conforme chegam.

3. **Micro-batch**  
   - Um meio-termo: dados são agrupados em pequenos lotes com pouca latência.  
   - Ex: Um job do Spark rodando a cada 30 segundos.

##### Fontes comuns de ingestão:

- Bancos de dados (MySQL, PostgreSQL, Oracle)
- APIs REST
- Arquivos CSV, Excel, JSON, Parquet
- Sistemas ERP ou CRM
- Fila de mensagens (Kafka, RabbitMQ)
- Sensores e dispositivos IoT
- Serviços de nuvem (Google Analytics, Facebook Ads, etc.)

##### Ferramentas populares para ingestão:

- **Batch**: Apache Nifi, Apache Sqoop, Talend, Airbyte  
- **Streaming**: Apache Kafka, Flink, Logstash, Kinesis  
- **Cloud**: AWS Glue, Google Cloud Dataflow, Azure Data Factory  

##### Exemplos práticos:

- Ingerir logs de acesso de um site para análise de tráfego.
- Puxar diariamente os leads do RD Station para um banco.
- Receber dados de temperatura de sensores industriais em tempo real.

##### Por que a ingestão é tão importante?

Porque **sem ingestão, não há dado para trabalhar**. É o primeiro passo para:
- Criar dashboards.
- Treinar modelos de machine learning.
- Fazer análises estatísticas.
- Automatizar decisões baseadas em dados.

---

#### Transformação e Enriquecimento

Depois da **ingestão**, os dados raramente estão prontos para uso. É aí que entram a **transformação** e o **enriquecimento** dos dados — duas etapas fundamentais na engenharia de dados.

##### 🔧 **Transformação de Dados**

**Transformar dados** significa alterar sua estrutura ou formato para que fiquem **mais limpos, padronizados, úteis e compatíveis com os objetivos do negócio**.

##### Exemplos de transformação:
- Converter datas para um único formato (ex: `01/04/2025` → `2025-04-01`)
- Trocar vírgulas por pontos em números decimais (`12,3` → `12.3`)
- Remover espaços em branco ou caracteres especiais
- Agrupar dados (ex: somar vendas por mês)
- Normalizar textos (`São Paulo`, `sao paulo`, `SÃO PAULO` → `Sao Paulo`)
- Criar colunas novas a partir de outras (ex: extrair o **ano** de uma data)

##### Ferramentas e técnicas:
- SQL (com `SELECT`, `CASE`, `CAST`, `JOIN`, etc.)
- Linguagens como Python (pandas), Scala, R
- Ferramentas ETL: dbt, Apache Spark, Airflow, etc.

##### ✨ **Enriquecimento de Dados**

**Enriquecer dados** é o processo de **adicionar novas informações a um conjunto de dados existente** para torná-lo mais completo, útil e valioso.

##### Exemplos de enriquecimento:
- Juntar dados de clientes com dados geográficos (CEP → cidade e estado)
- Incluir a cotação do dólar em uma tabela de exportações
- Associar um ID de produto com sua descrição e categoria
- Usar uma API externa para descobrir a localização exata de um IP

#### Fontes para enriquecimento:
- Outras tabelas internas (via `JOIN`)
- APIs externas (Google Maps, Receita Federal, redes sociais)
- Catálogos de dados públicos

##### Resumindo:

| Etapa           | O que faz                                 | Exemplo prático                            |
|----------------|--------------------------------------------|---------------------------------------------|
| **Transformação** | Limpa e organiza os dados                 | Corrigir formatação de datas, normalizar texto |
| **Enriquecimento** | Adiciona novas informações úteis          | Trazer o nome do cliente a partir de um ID  |

Ambas as etapas são parte do famoso processo **ETL (Extract, Transform, Load)**, e são essenciais para garantir que os dados estejam **confiáveis, completos e prontos para análise** ou machine learning.

---

#### Carga e Uso dos Dados

Depois de **ingestão**, **transformação** e **enriquecimento**, o ciclo é fechado com as fases finais do pipeline de engenharia de dados: **Carga (Load)** e **Uso dos Dados**.

##### 🚚 **Carga (Load)**

A **carga de dados** é o processo de **salvar os dados transformados e enriquecidos no destino final**, que pode ser:

- Um **Data Lake** (armazenamento bruto em grande escala, ex: S3, Google Cloud Storage)
- Um **Data Warehouse** (estrutura otimizada para análises, ex: BigQuery, Redshift, Snowflake)
- Um **banco de dados relacional** (ex: PostgreSQL, MySQL, SQL Server)
- Uma ferramenta de **BI** (ex: Power BI, Tableau)
- Um sistema de terceiros (ex: CRM, ERP, API externa)

##### Tipos de carga:
- **Full Load (carga total)**: os dados antigos são apagados e substituídos por uma versão atualizada.
- **Incremental Load (carga incremental)**: apenas os dados novos ou modificados são adicionados/atualizados.

##### Exemplo:
Após transformar uma planilha de vendas, os dados são carregados para um banco PostgreSQL, onde os analistas poderão consultá-los.

##### 📊 **Uso dos Dados**

Depois da carga, os dados estão prontos para serem **consumidos**. Isso é onde eles finalmente **geram valor para o negócio**.

##### Principais formas de uso:

1. **Business Intelligence (BI)**  
   - Ferramentas como Power BI, Tableau e Looker acessam os dados e criam dashboards, KPIs e relatórios interativos.
   - Ex: visualizar vendas por região e por período.

2. **Data Science / Machine Learning**  
   - Cientistas de dados usam os dados carregados para treinar modelos, prever comportamentos ou detectar anomalias.
   - Ex: prever churn de clientes ou demanda futura.

3. **APIs e produtos de dados**  
   - Os dados podem alimentar APIs internas ou produtos digitais (apps, sistemas, etc.).
   - Ex: uma API de recomendação de produtos baseada em dados de comportamento.

4. **Tarefas operacionais / automações**  
   - Robôs, scripts ou sistemas usam os dados para tomar decisões ou realizar ações automaticamente.
   - Ex: disparar um e-mail de alerta quando um estoque estiver abaixo do mínimo.

##### 🔄 Onde isso entra no fluxo geral?

```text
[Ingestão] → [Transformação] → [Enriquecimento] → [Carga] → [Uso dos Dados]
```

Cada etapa é dependente da anterior, e todas fazem parte do **pipeline de dados**.

#### Exemplo prático

**Mini pipeline de engenharia de dados**, simulando um cenário real simples — passo a passo — desde a ingestão até o uso.

##### 🧪 **Cenário fictício: Análise de vendas de uma loja online**

Você trabalha como engenheiro de dados e precisa entregar dados de vendas diárias limpos e prontos para o time de BI gerar relatórios.

###### ✅ 1. **Ingestão**

**Fonte de dados**:  
Um sistema da loja exporta diariamente um arquivo CSV com as vendas do dia, como este:

```
id,produto_id,data_venda,preco,quantidade
1,101,27/04/2025,49,90,1
2,102,27/04/2025,129.99,2
```

Você cria um script que **lê esse arquivo todo dia às 23h** e o envia para o pipeline.

Ferramentas possíveis: Python (`pandas`), Apache Airflow (agendamento), Shell Script.

##### 🔧 2. **Transformação**

Você nota problemas:
- O campo `data_venda` está no formato errado.
- O campo `preco` vem com vírgula ao invés de ponto (ex: `49,90`).
- Precisa de uma nova coluna: **valor_total** (preço × quantidade)

Você transforma os dados assim:

```python
import pandas as pd

df = pd.read_csv("vendas.csv", sep=",")
df['data_venda'] = pd.to_datetime(df['data_venda'], dayfirst=True)
df['preco'] = df['preco'].astype(str).str.replace(',', '.').astype(float)
df['valor_total'] = df['preco'] * df['quantidade']
```

##### ✨ 3. **Enriquecimento**

Você tem uma outra tabela de produtos:

```csv
produto_id,nome,categoria
101,Camisa Polo,Roupas
102,Smartwatch,Eletrônicos
```

Você faz um **JOIN** para adicionar nome e categoria do produto:

```python
produtos = pd.read_csv("produtos.csv")
df = df.merge(produtos, on='produto_id', how='left')
```

Agora seu dataframe tem:

```
id | produto_id | data_venda | preco | quantidade | valor_total | nome         | categoria
1  | 101        | 2025-04-27 | 49.90 | 1          | 49.90       | Camisa Polo  | Roupas
```

##### 🚚 4. **Carga**

Você salva os dados prontos no seu **Data Warehouse** (ex: PostgreSQL ou BigQuery):

```python
from sqlalchemy import create_engine

engine = create_engine("postgresql://user:senha@host:porta/banco")
df.to_sql("vendas_processadas", engine, if_exists="append", index=False)
```

##### 📊 5. **Uso dos Dados**

Agora, o time de BI conecta o Power BI diretamente na tabela `vendas_processadas`, e pode:

- Ver **vendas por categoria**
- Comparar **faturamento por período**
- Criar **gráficos e dashboards**

##### 🔄 Resumo visual:

```text
📁 CSV de vendas (raw)
 ↓ ingestão
🧹 Limpeza + Cálculo de total
 ↓ transformação
➕ Adição de nome e categoria do produto
 ↓ enriquecimento
💾 Salva no PostgreSQL
 ↓ carga
📊 Usado no Power BI para relatórios
```

---

#### Armazenamento

Vamos entender **onde entra o armazenamento** no pipeline de dados e **quais tipos existem**, com foco na etapa de **carga** e posterior **uso dos dados**.

##### 🗄️ **Armazenamento no pipeline de dados**

O **armazenamento** é onde os dados ficam **guardados em cada etapa** do processo — desde os brutos até os prontos para análise.

Ele pode ocorrer em **várias camadas diferentes**, dependendo da arquitetura adotada. Veja abaixo:

##### 🧱 Tipos de armazenamento no pipeline:

##### 1. **Raw Layer (Camada Bruta)**
- **O que é**: onde os dados são armazenados *logo após a ingestão*, sem transformação.
- **Usos**: backup, reprocessamento, auditoria.
- **Formato**: arquivos CSV, JSON, Parquet, Avro.
- **Exemplos**:
  - Amazon S3
  - Google Cloud Storage
  - Azure Blob Storage
  - HDFS (Hadoop)

##### 2. **Staging Layer (Camada Intermediária ou Temporária)**
- **O que é**: onde os dados **transformados parcialmente** ficam antes do armazenamento final.
- **Usos**: etapa de preparação, controle de qualidade, testes.
- **Formato**: tabelas temporárias, arquivos organizados.
- **Exemplos**:
  - Tabelas staging no PostgreSQL, BigQuery, etc.
  - Pastas intermediárias no S3

##### 3. **Curated / Trusted Layer (Camada Curada ou Confiável)**
- **O que é**: onde os dados **prontos para análise** são armazenados.
- **Usos**: BI, relatórios, ML, dashboards.
- **Formato**: tabelas bem definidas, padronizadas.
- **Exemplos**:
  - Data Warehouse (Redshift, BigQuery, Snowflake, PostgreSQL)
  - Data Mart (subconjunto voltado para áreas específicas)

##### 4. **Serving Layer (Camada de Consumo)**
- **O que é**: onde os dados são expostos diretamente para o consumidor final (BI, API, apps).
- **Usos**: dashboards, relatórios, sistemas externos.
- **Formato**: otimizado para consultas rápidas.
- **Exemplos**:
  - Power BI, Tableau (conectados ao DW)
  - APIs com dados consolidados
  - Dashboards internos

##### 🏗️ Arquitetura de exemplo com armazenamento:

```text
[Arquivo CSV] → S3 (raw)
           ↓
 Transformação → Tabela staging no PostgreSQL
           ↓
 Enriquecimento → Tabela final no Data Warehouse
           ↓
 BI → Power BI conectado ao DW
```

##### 📌 Conclusão:

O **armazenamento** é essencial em cada etapa:
- **Antes da transformação**: guardar o dado cru.
- **Durante**: salvar temporariamente os dados sendo processados.
- **Depois**: guardar os dados confiáveis, prontos para consumo.

Ele garante **resiliência, rastreabilidade, e performance** do pipeline.

---

