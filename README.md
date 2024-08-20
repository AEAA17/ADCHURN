# Churn

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
