# Pipeline de dados - Combustíveis Brasil

O objetivo desse projeto foi criar um pipeline de dados para extrair informações de uma base de dados pública acerca do preço dos combustíveis no Brasil. 

- **Fonte de Dados:** 
  - Site: https://dados.gov.br/dataset/serie-historica-de-precos-de-combustiveis-por-revenda
  - Metadados: https://www.gov.br/anp/pt-br/centrais-de-conteudo/dados-abertos/arquivos/shpc/metadados-serie-historica-precos-combustiveis.pdf


Os componentes foram organizados na seguinte arquitetura: 

![alt text](./img/combustiveis_brasil.png)

Para o Airflow, foi utilizada uma imagem docker que está no diretório /airflow. Entretanto, para  subir o serviço localmente basta executar o comando **make** a partir do diretório raiz. 

O pipeline em PySpark seguiu os seguintes passos: 

A função main recebe como parâmetro:
- path_input: Caminho dos dados no GCS gerados pela API coletora. Ex: gs://bucket_name/file_name.
- path_output: Caminho de onde será salvo os dados processados. Ex: gs://bucket_name_2/file_name.
- formato_file_save: Formato de arquivo a ser salvo no path_output. Ex: PARQUET.
- dataset: Dataset no BigQuery onde está a tabela.
- tabela_bq: Tabela do BigQuery que será salvo os dados. Ex: dataset.tabela_exemplo

Quanto a transformação de dados, foram realizadas as seguintes etapas: 

1. Faça a leitura dos dados de acordo com o path_input informado
2. Realize o rename de colunas do arquivo, respeitando os padrões do BigQuery
3. Adicione uma coluna de Ano, baseado na coluna `Data da Coleta`
4. Adicione uma coluna de Semestre, baseado na coluna de `Data da Coleta`
5. Adicione uma coluna Filename. Tip: pyspark.sql.functions.input_file_name
6. Faça o parse dos dados lidos de acordo com a tabela no BigQuery
7. Escreva os dados no Bucket GCS, no caminho informado `path_output`
no formato especificado no atributo `formato_file_save`.
8. Escreva os dados no BigQuery de acordo com a tabela especificada no atributo `tabela_bq`.

Segue o link para o Dashboard: 
[Link](https://datastudio.google.com/reporting/1c573150-7a59-4e89-9708-d2e8b1b14d62/page/1M) para o dashboard.

![alt text](./img/dashboard.png)
