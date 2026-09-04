# Checkpoint 1: Análise Exploratória de Dados

Este projeto apresenta uma **Análise Exploratória de Dados (EDA)** sobre vendas de uma rede fictícia de lojas de roupas masculinas. A análise consolida transações realizadas entre 2018 e 2022 e busca transformar os registros brutos em informações úteis para compreender produtos, preços, sazonalidade, canais de venda e comportamento dos clientes.

## Objetivos

- Consolidar os arquivos anuais em uma única base de análise.
- Avaliar a qualidade, a consistência e a estrutura dos dados.
- Identificar problemas de duplicidade, valores extremos e inconsistências de categoria.
- Normalizar as categorias de produtos para permitir comparações confiáveis.
- Analisar preços, volume de vendas, receita e sazonalidade.
- Comparar zonas e canais de venda.
- Avaliar a recorrência de compra dos clientes.

## Estrutura do projeto

```text
checkpoint/
├── README.md
└── checkpoint_1/
	 ├── requirements.txt
	 ├── data/
	 │   ├── data_2018.csv
	 │   ├── data_2019.csv
	 │   ├── data_2020.csv
	 │   ├── data_2021.csv
	 │   └── data_2022.csv
	 └── notebooks/
		  └── eda_checkpoint_1.ipynb
```

## Datasets

Os dados estão distribuídos em cinco arquivos CSV, um para cada ano do período analisado. Juntos, os arquivos possuem aproximadamente **8,45 milhões de registros**.

| Arquivo | Período | Registros |
|---|---:|---:|
| `data_2018.csv` | 2018 | 1.442.473 |
| `data_2019.csv` | 2019 | 1.616.186 |
| `data_2020.csv` | 2020 | 1.200.537 |
| `data_2021.csv` | 2021 | 1.661.026 |
| `data_2022.csv` | 2022 | 2.534.161 |

Os arquivos possuem a mesma estrutura e podem ser concatenados verticalmente. A coluna `year` identifica o ano de referência de cada registro.

### Fonte e download dos dados

Os arquivos originais podem ser baixados no dataset disponibilizado pelo Kaggle:

[Retail Store Dataset - Kaggle](https://www.kaggle.com/datasets/nishchay331/retail-store)

Após o download, coloque os arquivos CSV na pasta `checkpoint_1/data/`. Os dados brutos não são versionados no GitHub, pois estão incluídos no `.gitignore` devido ao seu tamanho.

### Dicionário de dados

| Coluna | Descrição |
|---|---|
| `user_id` | Identificador do cliente. |
| `bill_id` | Identificador da nota ou transação. |
| `line_item_amount` | Valor registrado para o item da linha da venda. |
| `bill_discount` | Desconto associado à nota fiscal. |
| `transaction_date` | Data da transação. |
| `description` | Descrição textual do produto ou item vendido. |
| `inventory_category` | Categoria original registrada no inventário. |
| `colour` | Cor do produto. |
| `size` | Tamanho do produto. |
| `zone_name` | Zona ou canal de venda. |
| `store_name` | Loja associada à transação. |
| `year` | Ano de referência do arquivo. |

## Notebook de EDA

O notebook [eda_checkpoint_1.ipynb](checkpoint_1/notebooks/eda_checkpoint_1.ipynb) documenta todo o processo de exploração. A análise segue estas etapas:

1. Importação das bibliotecas e leitura dos arquivos anuais.
2. Concatenação dos dados em um único DataFrame.
3. Inspeção inicial, análise de nulos e investigação de duplicatas.
4. Estudo das repetições de cliente, nota e produto para identificar uma possível quantidade implícita.
5. Validação de datas, preços, descontos e valores extremos.
6. Limpeza dos registros e correção de valores afetados por erro de casa decimal.
7. Normalização das categorias a partir dos prefixos das descrições e dos códigos de produto.
8. Separação entre itens de vestuário e itens não relacionados a roupas.
9. Análises de preço, categorias, tendência mensal e sazonalidade.
10. Comparação por zona e análise de recorrência dos clientes.

## Principais cuidados metodológicos

### Duplicatas e quantidade comprada

Registros repetidos com a mesma combinação de cliente, nota e descrição não são removidos automaticamente. A repetição pode representar a compra de várias unidades do mesmo item, especialmente no caso de produtos não relacionados a vestuário.

### Valores extremos

Alguns valores muito altos de `line_item_amount` são investigados como possível erro de escala. O tratamento é aplicado somente aos registros compatíveis com a hipótese de erro de casa decimal, preservando os demais dados para auditoria.

### Categorias de produto

`inventory_category` apresenta muitas variações de nomenclatura. Por isso, a EDA cria a coluna `category_clean`, usando prefixos da descrição e padrões dos SKUs para formar categorias mais consistentes.

### Itens de vestuário e não-vestuário

A coluna `is_apparel_v2` é baseada na categoria normalizada. Ela oferece uma separação mais confiável entre roupas e itens como sacolas, vales-presente, acessórios e materiais de loja do que uma classificação baseada apenas em palavras da descrição.

## Principais perguntas respondidas

- Quais categorias de vestuário possuem maior volume de vendas?
- Como os preços se distribuem entre as categorias?
- Quais meses concentram maior volume de vendas?
- Como a pandemia afetou a receita e o volume de transações?
- Quais zonas ou canais apresentam maior receita e ticket médio?
- Qual é a proporção de clientes ocasionais e recorrentes?
- O mix de categorias varia entre as diferentes zonas de venda?

## Como executar

1. Instale o Python e o Jupyter Notebook.
2. Na pasta `checkpoint/checkpoint_1`, instale as dependências:

	```bash
	pip install -r requirements.txt
	```

3. Abra o notebook:

	```bash
	jupyter notebook notebooks/eda_checkpoint_1.ipynb
	```

O notebook utiliza caminhos relativos à pasta `notebooks`. Por isso, a estrutura de diretórios deve ser mantida para que os arquivos em `data/` sejam encontrados corretamente.

## Tecnologias utilizadas

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Jupyter Notebook

## Observação

Os dados pertencem a um cenário fictício e são utilizados exclusivamente para fins acadêmicos. As conclusões da EDA devem ser interpretadas como análises exploratórias do conjunto fornecido, e não como indicadores de uma operação comercial real.
