People Analytics Pipeline: API Elofy to Data Lakehouse 🚀
Este projeto demonstra a construção de um pipeline de dados de ponta a ponta utilizando Databricks e Spark, seguindo a Arquitetura Medallion (Bronze, Silver e Gold). O objetivo é extrair dados de colaboradores da API da Elofy, tratar inconsistências e gerar indicadores de RH.

🛠️ Tecnologias Utilizadas
Linguagem: Python (PySpark)

Plataforma: Databricks (Community Edition)

Armazenamento: Delta Lake (ACID Transactions)

Orquestração: Databricks Notebook Workflows (dbutils)

🏗️ Arquitetura do Projeto
O pipeline foi estruturado em três camadas para garantir a linhagem e a qualidade dos dados:

Camada Bronze (Raw): * Ingestão via API REST com autenticação Bearer Token.

Validação de integridade: Filtro de registros sem login ou id_usuario.

Carga incremental utilizando o comando MERGE (Upsert) para evitar duplicidade.

Camada Silver (Cleansed):

Tratamento de tipos de dados (Casting).

Data Parsing: Conversão de strings de data no formato brasileiro (dd/MM/yyyy) para o padrão Spark DateType.

Padronização de strings (Upper Case em campos de Status).

Limpeza de IDs flutuantes vindos do JSON original.

Camada Gold (Curated):

Criação de métricas de negócio.

Cálculo de Tenure (Tempo de casa em meses).

Classificação de perfil de retenção (Novato, Estabilizado, Veterano).

⚙️ Orquestração
Devido às limitações da versão Community do Databricks para agendamento de Jobs, foi desenvolvido um Notebook Orquestrador (00_Pipeline_Usuarios). Ele gerencia as dependências entre as camadas, garantindo que a transformação (Silver) só ocorra após o sucesso da ingestão (Bronze).

Python
# Exemplo da lógica de orquestração utilizada
dbutils.notebook.run("./01_Bronze_table_Usuarios", 600)
dbutils.notebook.run("./02_Silver_table_Usuarios", 600)
dbutils.notebook.run("./03_Gold_table_Usuarios", 600)
📈 Insights Gerados
A tabela final na camada Gold permite análises imediatas como:

Distribuição de colaboradores por tempo de empresa.

Identificação de talentos em risco de turnover.

Visão consolidada de cargos e times para Dashboards de BI.
