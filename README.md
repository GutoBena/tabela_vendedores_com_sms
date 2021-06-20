# tabela_vendedores_com_sms
# Programa onde verifica em uma base de dados ( Excel ) algum vendedor obteve a venda maior que 55000.
# Esta tabela deve estar no mesmo projeto # E envia um SMS avisando que alguém obteve a condição de venda > 550000


import pandas as pd
from twilio.rest import Client

# Your Account SID from twilio.com/console
account_sid = "AC8ac6304b20fdc65556089bc1b530f74b"
# Your Auth Token from twilio.com/console
auth_token  = "286c8bd8612cfe32ef36a1aaef68e1e3"
client = Client(account_sid, auth_token)

# Abrir os 6 arquivos em Excel
lista_meses = ['janeiro', 'fevereiro', 'março', 'abril', 'maio','junho']  # Variável lista de mêses

for mes in lista_meses:
    #print(mes)
    tabela_vendas = pd.read_excel(f'{mes}.xlsx')  # Abrindo as planilhas
   # print(tabela_vendas)
    if (tabela_vendas['Vendas'] > 55000).any():
        vendedor = tabela_vendas.loc[tabela_vendas['Vendas'] > 55000, 'Vendedor'].values[0]                 # Localizando na tabelas
        vendas =  tabela_vendas.loc[tabela_vendas['Vendas'] > 55000, 'Vendas'].values[0]
        print(f' No mês {mes} alguém bateu a meta. Vendedor : {vendedor}, Vendas: {vendas}')
        message = client.messages.create(
            to="+5512996492969",
            from_="+13128182507",
            body=f' No mês {mes} alguém bateu a meta. Vendedor : {vendedor}, Vendas: {vendas}') # Mensagem SMS
        print(message.sid)


# link twilio : https://www.twilio.com/docs/libraries/python
