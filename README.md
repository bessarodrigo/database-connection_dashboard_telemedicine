# Integração de Dados e Visualização - Empresa de Telemedicina

## Visão Geral
Projeto que tem como objetivo **automatizar o processo de ingestão e visualização de dados** para uma operação de uma empresa de telemedicina (considerando os dados de produtos e da área financeira), utilizando ferramentas de ETL, armazenamento e visualização. A arquitetura é escalável, documentada e organizada para fins de portfólio profissional.

## Arquitetura 

### Fonte dos Dados: planilhas Excel com múltiplas abas

### ETL com Python
- Leitura e tratamento dos dados usando pandas
- Conversão e padronização de colunas (datas, numéricos, strings)
- Estrutura modular com função upload_to_bigquery_from_excel reutilizável
- so de variáveis de ambiente com dotenv para manter segurança e portabilidade

### Carga no Banco de Dados
- **PostgreSQL:** utilizado como banco relacional local em fase inicial
- **Google BigQuery:** usado como armazenamento em nuvem final, estruturado por datasets: seguros, financeiro, saude
- Uso da biblioteca **google-cloud-bigquery** com LoadJobConfig para definição de schemas e controle de sobrescrita com WRITE_TRUNCATE

### Visualização
- Conexão direta do BigQuery com o Looker Studio

## Tecnologias Utilizadas
| Ferramenta                  | Papel                              |
| --------------------------- | ---------------------------------- |
| **Python (pandas, dotenv)** | ETL, tratamento e envio dos dados  |
| **PostgreSQL**              | Banco relacional local             |
| **Google BigQuery**         | Armazenamento analítico e consulta |
| **Looker Studio**           | Painéis e visualização             |
| **Jupyter Notebook**        | Desenvolvimento interativo         |
| **GitHub**                  | Controle de versão e documentação  |


