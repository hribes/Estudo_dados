import pandas as pd
import matplotlib.pyplot as plt
from sqlalchemy import create_engine, inspect, text

url_itens_pedidos = 'https://github.com/alura-cursos/SQL-python-integracao/raw/main/TABELAS/itens_pedidos.csv'
url_pedidos = 'https://github.com/alura-cursos/SQL-python-integracao/raw/main/TABELAS/pedidos.csv'
url_produto = 'https://github.com/alura-cursos/SQL-python-integracao/raw/main/TABELAS/produtos.csv'
url_vendedores = 'https://github.com/alura-cursos/SQL-python-integracao/raw/main/TABELAS/vendedores.csv'

itens_pedidos = pd.read_csv(url_itens_pedidos)
pedidos = pd.read_csv(url_pedidos)
produto = pd.read_csv(url_produto)
vendedores = pd.read_csv(url_vendedores)

engine = create_engine('sqlite:///:memory:')

itens_pedidos.to_sql('itens_pedido', engine, index=False)
produto.to_sql('produto', engine, index=False)
pedidos.to_sql('pedido', engine, index=False)
vendedores.to_sql('vendedores', engine, index=False)

#Aqui estamos transformando nossas tabelas arquivos sql, com isso os enviamos para a nossa engine criada anteriormente
#Estamos enviando nossas tabelas para a engine sem o indice

def sql_df(query):
  with engine.connect() as conexao:
    consulta = conexao.execute(text(query))
    dados = consulta.fetchall()
  return pd.DataFrame(dados, columns=consulta.keys())

query = '''SELECT SUM(quantidade * valor_unitario) AS 'Receita_total'
FROM ITENS_PEDIDO'''

receita_total = sql_df(query)
print(receita_total['Receita_total'][0]) #Receita total

query = '''SELECT PRODUTO.PRODUTO, SUM(ITENS_PEDIDO.quantidade * ITENS_PEDIDO.valor_unitario) AS 'Receita_produto'
FROM ITENS_PEDIDO, PRODUTO
WHERE PRODUTO.PRODUTO_ID = ITENS_PEDIDO.PRODUTO_ID
GROUP BY PRODUTO.PRODUTO
ORDER BY Receita_produto ASC'''

receita_produto = sql_df(query)
print(receita_produto)

#Grafico
plt.barh(receita_produto['produto'][-10:], receita_produto['Receita_produto'][-10:], color='#9353FF')
plt.xlabel('Receita em Milhão')
plt.title('Os 10 produtos com maior receita')
plt.show()

query = '''SELECT PRODUTO.MARCA, SUM(ITENS_PEDIDO.quantidade * ITENS_PEDIDO.valor_unitario) AS 'Receita_marca'
FROM ITENS_PEDIDO, PRODUTO
WHERE PRODUTO.PRODUTO_ID = ITENS_PEDIDO.PRODUTO_ID
GROUP BY PRODUTO.MARCA
ORDER BY Receita_marca ASC'''

receita_marca = sql_df(query)
print(receita_marca)

plt.barh(receita_marca['marca'][-15:], receita_marca['Receita_marca'][-15:], color='#9353FF')
plt.xlabel('Receita em Milhão')
plt.title('As 15 marcas com maiores receitas')
plt.show()

query = '''SELECT PRODUTO.MARCA, SUM(ITENS_PEDIDO.QUANTIDADE) AS Quantidade
FROM ITENS_PEDIDO, PRODUTO
WHERE ITENS_PEDIDO.PRODUTO_ID = PRODUTO.PRODUTO_ID
GROUP BY PRODUTO.MARCA
ORDER BY Quantidade ASC'''
#Apenas acrescentado no código anterior o ORDER BY
#Com ele iremos ordenar nossa tabela com base na quantidade e em ordem decrescente

df_marca_quant = sql_df(query)
print(df_marca_quant)	

plt.barh(df_marca_quant['marca'][-15:], df_marca_quant['Quantidade'][-15:], color='#9353FF')
plt.xlabel('Quantidade vendida')
plt.title('Os 15 marcas mais vendidos')
plt.show()
