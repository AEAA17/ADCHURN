Este código realiza uma análise exploratória de dados (EDA) para uma empresa de telecomunicações, com o objetivo de entender os fatores que influenciam o churn, que é a saída de clientes da empresa. A análise se concentra em variáveis numéricas e categóricas do conjunto de dados, a fim de identificar padrões e possíveis correlações com o churn.

1. Importação das Bibliotecas
As bibliotecas pandas, seaborn e matplotlib foram importadas para manipulação de dados e visualização. O tema de seaborn foi configurado com estilo "dark", paleta "bright", e contexto "notebook", para um visual agradável e legível.

2. Carregamento e Tratamento dos Dados
O arquivo de dados churn_clientes.csv foi carregado em um DataFrame df_churn. A coluna id_cliente, que não é relevante para a análise, foi removida.

3. Separação das Variáveis
As variáveis do DataFrame foram divididas em numéricas e categóricas. As variáveis numéricas foram selecionadas com base no tipo de dado (number), enquanto as variáveis categóricas foram identificadas como aquelas que não são do tipo number, excluindo a variável alvo churn.

4. Análise das Variáveis Numéricas
Foram criados histogramas para visualizar a distribuição das variáveis numéricas em relação ao tipo de contrato, permitindo observar como diferentes contratos influenciam essas variáveis. Em seguida, foram plotados gráficos de densidade (KDE) para visualizar a distribuição das variáveis numéricas em relação ao churn, possibilitando a identificação de como as distribuições variam entre clientes que permaneceram e os que saíram.

5. Análise das Variáveis Categóricas
Para as variáveis categóricas, foram criados gráficos de barras utilizando histogramas que mostram a distribuição percentual do churn dentro de cada categoria. Cada gráfico exibe a proporção de clientes que saíram ou permaneceram, com rótulos de percentual centralizados nas barras para facilitar a interpretação visual.

6. Criação de Dummies e Análise de Correlação
A variável churn foi convertida para valores binários, onde 'Sim' foi substituído por 1 e 'Não' por 0. Em seguida, foram criadas variáveis dummy para as variáveis categóricas. A correlação entre essas variáveis dummy e o churn foi calculada e visualizada em um gráfico de barras, ordenando as variáveis pela correlação mais alta com o churn. Isso ajuda a identificar quais fatores têm maior impacto na saída de clientes.



Código:

# Importação das Bibliotecas

import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

sns.set_theme(style='dark', palette='bright', context='notebook')

# Carregamento dos dados e tratamento de dados
ARQUIVO_DADOS = 'churn_clientes.csv'

df_churn = pd.read_csv(ARQUIVO_DADOS)
df_churn = df_churn.drop(columns='id_cliente', axis=1)
display(df_churn)

# Divisão de variaveis numericas e categoricas
colunas_numericas = df_churn.select_dtypes(include='number').columns
colunas_categoricas = df_churn.select_dtypes(exclude='number').columns
colunas_categoricas = colunas_categoricas.drop('churn')

# Análise das Variáveis Numéricas

# Visualização das distribuições das variáveis numéricas com histogramas
fig, axs = plt.subplots(nrows=1, ncols=3, figsize=(14,6))
fig.suptitle('Distribuição do churn de acordo com o tipo de contrato\n\n', fontsize=18)
for i, coluna in enumerate(colunas_numericas):
    sns.histplot(x=coluna, data=df_churn, hue='contrato', multiple='stack', ax=axs[i], bins=15)
plt.show()


# Gráficos de densidade (KDE) das variáveis numéricas em relação ao churn
fig, axs = plt.subplots(nrows=1, ncols=3, figsize=(14,6), tight_layout=True)
for i, coluna in enumerate(colunas_numericas):
    sns.kdeplot(x=coluna, data=df_churn, hue='churn', ax=axs[i], fill=True)
    legenda = axs[i].get_legend()
    legenda.remove()

rotulos = [text.get_text() for text in legenda.get_texts()]
fig.legend(handles=legenda.legend_handles, labels=rotulos, loc='upper center', ncols=2, title='Churn', bbox_to_anchor=(0.13,0.82))
fig.suptitle('Distribuição das variáveis numéricas de acordo com o churn\n\n', fontsize=18)
plt.show()

# Análise das Variáveis Categóricas

from matplotlib.ticker import PercentFormatter

fig, axs = plt.subplots(nrows=4, ncols=4, figsize=(14, 16), sharey=True)

for i, coluna in enumerate(colunas_categoricas):
    h = sns.histplot(x=coluna, hue='churn', data=df_churn, multiple='fill', ax=axs.flat[i], stat='percent',
                     shrink=0.8)
    h.tick_params(axis='x', labelrotation=45)
    h.grid(False)

    h.yaxis.set_major_formatter(PercentFormatter(1))
    h.set_ylabel('')

    for bar in h.containers:
        h.bar_label(bar, label_type='center', labels=[f'{b.get_height():.1%}' for b in bar], color='white', weight='bold', fontsize=11)

    legend = h.get_legend()
    legend.remove()

labels = [text.get_text() for text in legend.get_texts()]

fig.legend(handles=legend.legend_handles, labels=labels, loc='upper center', ncols=2, title='Churn', bbox_to_anchor=(0.5, 0.965))
fig.suptitle('Churn por variável categórica', fontsize=16)

fig.align_labels()

plt.subplots_adjust(wspace=0.4, hspace=0.4, top=0.925)

plt.show()

# Criação de Dummies 

# Gráfico de barras para análise da correlação das variáveis
df_churn['churn'] = df_churn['churn'].replace({'Sim': 1, 'Não': 0})
df_dummies = pd.get_dummies(df_churn)
churn_c = df_dummies.corr()['churn'].sort_values(ascending=False)
fig, ax = plt.subplots(figsize=(16,6))
b = sns.barplot(x=churn_c[1:].index, y=churn_c[1:].values, palette='coolwarm', ax=ax)
b.tick_params(axis='x', rotation=90);
plt.show()
