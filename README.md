# Pipeline de Ingestão Salesforce -> Databricks Lakehouse (Medallion Architecture)

Este repositório contém a implementação de um pipeline de dados end-to-end para ingestão incremental, limpeza e agregação de dados do CRM Salesforce no Databricks utilizando **PySpark**, **Delta Lake** e **Unity Catalog**.

---

##  Arquitetura da Solução

O pipeline segue o padrão de **Arquitetura Medallion**:

1. **Camada Bronze (`01_ingest_bronze`):**
   * Extração incremental via **Salesforce Bulk API 2.0** utilizando o mecanismo de *Watermark* (`SystemModstamp`).
   * Armazenamento do dado bruto com metadados de auditoria (`_ingested_at`, `_source`) em formato Delta Lake.

2. **Camada Silver (`02_bronze_to_silver`):**
   * Limpeza de caracteres especiais e padronização de campos de texto (`StageName`).
   * Tratamento de valores nulos e deduplicação de registros via chave primária (`Id`).

3. **Camada Gold (`03_silver_to_gold`):**
   * Agregação e construção de KPIs de negócios (total acumulado e contagem de oportunidades por estágio) prontos para consumo por ferramentas de BI (Power BI/Tableau).

---

##  Tecnologias Utilizadas

* **Linguagens:** PySpark (Python) / SQL
* **Platform:** Databricks (Unity Catalog, Delta Lake)
* **Orquestração:** Databricks Workflows (Job com execução sequencial diária)
* **Controle de Versão:** Git / GitHub (Databricks Git Folders)

---

##  Estrutura do Repositório

```text
Portfolio_DataBricks/
│
├── src/
│   ├── 01_ingest_bronze.py       # Extração e carga bruta (Bronze)
│   ├── 02_bronze_to_silver.py    # Limpeza, deduplicação e tratamento (Silver)
│   └── 03_silver_to_gold.py      # Agregações e regras de negócio (Gold)
│
└── README.md                     # Documentação do projeto
