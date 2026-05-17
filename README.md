# Projeto Semantix - Análise de Atrasos em Entregas Olist

## 1. Contexto do projeto

Este projeto foi desenvolvido com o objetivo de analisar os fatores associados aos atrasos nas entregas de pedidos da base pública da Olist e construir modelos preditivos capazes de identificar pedidos com maior risco de atraso.

A análise considera informações relacionadas aos pedidos, clientes, vendedores, categorias de produtos, valores de produtos, frete, avaliações dos clientes e localização dos envolvidos na operação.

O principal objetivo não é afirmar que os atrasos representam a maior parte dos pedidos, mas entender em quais situações eles ocorrem, quais fatores estão associados a eles e como esses atrasos impactam a experiência do cliente.

## 2. Base de dados

A base utilizada foi o conjunto de dados público da Olist, disponível no Kaggle. Trata-se de uma base real de e-commerce brasileiro, contendo informações de pedidos, clientes, vendedores, produtos, pagamentos, avaliações e geolocalização.

Foram utilizadas principalmente informações de:

- pedidos;
- clientes;
- vendedores;
- itens dos pedidos;
- produtos;
- avaliações dos clientes;
- categorias de produtos.

A base é pública e não contém dados confidenciais diretamente identificáveis dos clientes.

## 3. Definição da variável alvo

A variável alvo do projeto foi criada a partir da comparação entre a data estimada de entrega e a data real de entrega.

Foram considerados:

- `0`: pedido entregue dentro do prazo;
- `1`: pedido entregue com atraso.

Dessa forma, o problema foi tratado como uma tarefa de classificação binária, com o objetivo de prever se um pedido possui risco de atraso.

## 4. Preparação dos dados

Durante a etapa de preparação, foram realizadas as seguintes atividades:

- leitura dos arquivos CSV;
- integração das bases;
- tratamento das variáveis de data;
- criação da variável de atraso;
- seleção das variáveis relevantes;
- criação de variáveis auxiliares, como rota entre estado do vendedor e estado do cliente;
- análise de valores ausentes;
- separação entre variáveis explicativas e variável alvo;
- pré-processamento das variáveis categóricas e numéricas.

As variáveis categóricas foram tratadas com OneHotEncoder e as variáveis numéricas foram padronizadas com StandardScaler.

## 5. Análise exploratória

A análise exploratória mostrou que a maior parte dos pedidos foi entregue dentro do prazo. Os pedidos com atraso representaram uma parcela menor da base, cerca de 6,77% do total analisado.

Apesar disso, os pedidos com atraso apresentaram diferenças importantes em relação a algumas variáveis, especialmente:

- tempo de entrega;
- estado do vendedor;
- estado do cliente;
- rotas entre vendedor e cliente;
- categoria do produto;
- valor do produto;
- valor do frete;
- avaliação do cliente.

A análise regional mostrou que os atrasos não devem ser avaliados apenas pelo percentual, mas também pelo volume absoluto de pedidos afetados. Alguns estados e rotas apresentaram percentuais maiores, mas com baixo volume de pedidos. Já outros concentraram maior quantidade absoluta de atrasos, indicando maior impacto operacional.

Na análise por categoria de produto, algumas categorias apresentaram maior proporção de atraso, mas muitas possuíam baixo volume. Ao considerar a quantidade absoluta de pedidos atrasados e a participação no total de atrasos, categorias como `bed_bath_table`, `health_beauty`, `sports_leisure`, `furniture_decor`, `computers_accessories` e `watches_gifts` se destacaram.

Também foi observado que pedidos com atraso apresentaram avaliações consideravelmente menores. Enquanto os pedidos dentro do prazo tiveram média de avaliação próxima de 4,29 e mediana igual a 5, os pedidos com atraso tiveram média próxima de 2,27 e mediana igual a 1.

## 6. Modelagem preditiva

Foram avaliados três modelos de classificação:

- Regressão Logística;
- Árvore de Decisão;
- Random Forest.

Como a variável alvo é desbalanceada, a avaliação não foi feita apenas pela acurácia. Também foram consideradas as métricas:

- precisão;
- recall;
- F1-score;
- ROC AUC;
- matriz de confusão;
- curva ROC.

O desbalanceamento da base foi tratado com o parâmetro `class_weight='balanced'`, para dar maior peso à classe minoritária, composta pelos pedidos com atraso.

## 7. Resultados dos modelos

Os modelos apresentaram desempenhos próximos. A Regressão Logística e o Random Forest tiveram resultados semelhantes, com F1-score próximo de 0,19 e ROC AUC em torno de 0,65.

A Árvore de Decisão apresentou recall ligeiramente maior para a classe de atraso, mas teve desempenho inferior em outras métricas, como acurácia e F1-score.

A matriz de confusão da Regressão Logística mostrou que o modelo conseguiu identificar parte dos pedidos atrasados, mas também gerou muitos falsos positivos, classificando como atraso pedidos que foram entregues dentro do prazo.

A curva ROC apresentou AUC de 0,65, indicando capacidade moderada de separação entre pedidos dentro do prazo e pedidos com atraso.

## 8. Modelo escolhido

A Regressão Logística foi escolhida como modelo de referência por apresentar desempenho semelhante ao Random Forest, mas com maior simplicidade e interpretabilidade.

O modelo não deve ser interpretado como uma ferramenta de decisão definitiva, mas pode ser utilizado como apoio à priorização logística, sinalizando pedidos com maior risco de atraso para análise complementar.

O modelo final foi salvo em arquivo `.pkl`, permitindo sua reutilização posterior.

## 9. Conclusão

O atraso não representa a maior parte dos pedidos analisados e, portanto, não deve ser tratado como um problema generalizado da operação. No entanto, quando ocorre, está associado a uma pior experiência do cliente, refletida principalmente nas avaliações mais baixas.

Os resultados mostram que o atraso possui relação mais clara com a satisfação do cliente do que com o volume geral de pedidos. Dessa forma, a análise dos atrasos é importante para reduzir avaliações negativas, melhorar a experiência de compra e apoiar decisões logísticas.

Os modelos desenvolvidos conseguiram capturar parte dos pedidos com maior risco de atraso, mas apresentaram limitações, principalmente em relação à precisão da classe de atraso. Isso indica que as variáveis disponíveis antes da finalização da entrega ajudam na previsão, mas não são suficientes para explicar completamente o atraso.

Para melhorias futuras, recomenda-se incluir novas variáveis logísticas e operacionais, como:

- distância real entre vendedor e cliente;
- transportadora;
- modalidade de envio;
- prazo prometido;
- histórico do vendedor;
- histórico da rota;
- tempo de separação;
- tempo de postagem;
- tempo de transporte.

Essas informações podem aumentar a capacidade preditiva dos modelos e permitir uma análise mais precisa das causas dos atrasos.

## 10. Estrutura do projeto

```text
projeto-semantix-olist/
│
├── notebooks/
│   └── projeto_semantix_olist.ipynb
│
├── output/
│   └── modelo_regressao_logistica.pkl
│
├── README.md
│
└── requirements.txt
