# Integração de Dados e Visualização - Empresa de Telemedicina

## Visão Geral
Projeto que tem como objetivo **automatizar o processo de ingestão e visualização de dados** para uma operação de uma empresa de telemedicina (considerando os dados de produtos e da área financeira), utilizando ferramentas de ETL, armazenamento e visualização. A arquitetura é escalável, documentada e organizada para fins de portfólio profissional.

## Arquitetura 

### Fonte dos Dados
- planilhas Excel com múltiplas abas

### ETL com Python
- Leitura e tratamento dos dados usando pandas
- Conversão e padronização de colunas (`DATE`, `FLOAT`, `STRING`)
- Estrutura modular com função upload_to_bigquery_from_excel reutilizável
- Uso de `.env` para variáveis de ambiente

### Carga no Banco de Dados
- **PostgreSQL:** utilizado como banco relacional local em fase inicial
- **Google BigQuery:** usado como armazenamento em nuvem final, estruturado por datasets: seguros, financeiro, saude
-  Carga usando API oficial do BigQuery com `LoadJobConfig`

### Visualização
- Conexão direta do BigQuery com o Looker Studio

## Estrutura do Projeto
```text
📁 projeto/
├── 📁 notebooks/
│   └── bq_connection_nomedatabela.ipynb
├── 📁 credenciais/
│   └── bq-chave.json
├── 📁 dados/
│   └── relatorio_consolidado.xlsx
├── .env
├── requirements.txt
└── README.md
```

## Tecnologias Utilizadas
| Ferramenta                  | Papel                              |
| --------------------------- | ---------------------------------- |
| **Python (pandas, dotenv)** | ETL, tratamento e envio dos dados  |
| **PostgreSQL**              | Banco relacional local             |
| **Google BigQuery**         | Armazenamento analítico e consulta |
| **Looker Studio**           | Painéis e visualização             |
| **Jupyter Notebook**        | Desenvolvimento interativo         |
| **GitHub**                  | Controle de versão e documentação  |

## Exemplos
---

### Envio de DataFrames para BigQuery

```python
def upload_to_bigquery_from_excel(sheet_name: str, table_name: str, schema: list):
    excel_path = os.getenv("EXCEL_FILE_PATH")
    if not excel_path or not os.path.exists(excel_path):
        raise FileNotFoundError("Arquivo Excel não encontrado ou variável EXCEL_FILE_PATH não definida.")
        
    df = pd.read_excel(excel_path, sheet_name=sheet_name)

    # Conversão de colunas do tipo DATE
    date_columns = [field.name for field in schema if field.field_type == "DATE"]
    for col in date_columns:
        df[col] = pd.to_datetime(df[col], errors='coerce')

    job_config = bigquery.LoadJobConfig(
        schema=schema,
        write_disposition="WRITE_TRUNCATE"
    )

    full_table_id = f"{project_id}.{dataset_id}.{table_name}"
    job = client.load_table_from_dataframe(df, full_table_id, job_config=job_config)
    job.result()

    print(f"✅ Dados enviados com sucesso para: {full_table_id}")

### Exemplo de Uso da Função
```python
schema_meta_odonto = [
    bigquery.SchemaField("Mes", "DATE"),
    bigquery.SchemaField("Meta", "INTEGER"),
]

upload_to_bigquery_from_excel(
    sheet_name='meta_odonto',
    table_name='meta_odonto',
    schema=schema_meta_odonto
)
