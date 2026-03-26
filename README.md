```markdown
# Previsão de Safra de Soja com Machine Learning: O Caso Sorriso-MT

> **Resumo:** Um modelo preditivo capaz de prever a produtividade da soja (kg/ha) na capital nacional do agronegócio, utilizando exclusivamente dados climáticos de satélite, atingindo uma precisão satisfatório dado a escassez dos dados.

---

## O Desafio
O estado de Mato Grosso é o coração da produção de soja no Brasil, mas a imprevisibilidade climática (como os efeitos severos do El Niño) gera incertezas para toda a cadeia produtiva. 

O objetivo deste projeto foi responder a uma pergunta central: **"É possível uma Inteligência Artificial prever a quebra ou o recorde de uma safra olhando apenas para o volume de chuva ao longo dos anos?"**

## A Matéria-Prima (Coleta de Dados)
Para construir este modelo, criei um pipeline de dados unindo duas fontes oficiais robustas:
* **NASA POWER (API):** Extração de dados históricos de precipitação pluviométrica (dataset muito interessante que só soube da existência por tentar achar dados bons/ não quebrados de precipitação).
* **IBGE (SIDRA):** 37 anos de histórico imaculado (1987-2023) da produtividade municipal em Sorriso-MT.

## Desenvolvimento
A princípio, após passar por uma jornada enriquecedora (e um tanto complexa) para conseguir dados valioso e inteiriços, e aplicar diversas técnicas de tratamento e manipulação dos dados,
pude realizar o primeiro treinamento de um modelo Random Forest (de regressão). Os resultados pra uma primeira tentativa foram "curiosos":
> Erro absoluto: 351.53 kg/ha
> R^2 Score: 1,9 %
Foi então que percebi que o que estava faltando um coisa crucial para os meus dados: regras de negócio.

## Feature Engineering Agronômico
O maior salto de precisão do modelo ocorreu quando deixei de tratar os dados como meros números e apliquei **conhecimento de negócio**. Traduzi o ciclo fenológico da soja para a IA, criando variáveis específicas:

1. **Chuva de Plantio (Set-Nov):** Onde a semente precisa germinar.
2. **Período de Seca (Jun-Ago):** Onde o déficit hídrico já é o padrão esperado.
3. **Enchimento de Grãos (Dez-Mar):** Fases reprodutivas (R5 e R6), cruciais para o peso final da vagem.

Dessa forma ao invés de um dataset com várias colunas separadas por meses, cheguei a um dataset definitivo que traduzia os intervalos de meses em que estavam ocorrendo o ciclo fenológico da soja.

## Resultados e Validação no Mundo Real (Safra 2024)
O modelo final alcançou resultados expressivos nos dados de teste, considerando que o que queria aqui era tentar prever algo que depende de diversas váriaveis,
mas a verdadeira prova de fogo foi tentar prever a **Safra de 2024** (um ano marcado por forte anomalia climática). Como o IBGE (SIDRA) disponibiliza até 2024, foi uma ótima oportunidade para ver como o modelo estava se saindo
em comparação a dados reais.

> Os resultados:

* **Erro Absoluto Médio (MAE):** ~283 kg/ha no histórico.
* **Previsão Cega (2024):** O modelo errou por apenas **141,61 kg/ha** (cerca de 2,3 sacas por hectare).

Os resultados foram em muito satisfatórios considerando as features usada (a feature no caso), um modelo que erra aproximadamente em 283 kg de soja por hectare, ou seja ~4,5 sacas de soja,
uma margem de 8% deerro considerando que a produtividade média recente em Mato Grosso passa dos 3.500 kg/ha (66 sacas).

Abaixo, o gráfico demonstra como a linha da IA acompanha a realidade do IBGE ao longo das décadas, detectando as quebras de safra:

![Gráfico comparativo entre IA e Realidade](grafico_previsao_soja_mt.png)

## Tecnologias Utilizadas
* **Python:** Linguagem principal
* **Pandas & NumPy:** Limpeza, formatação e Feature Engineering
* **Scikit-Learn:** Separação de dados e treinamento do Random Forest
* **Matplotlib & Seaborn:** Data Storytelling e visualização
* **Requests:** Consumo de APIs REST (NASA e IBGE)

---
*Projeto desenvolvido para estudo e aplicação de Data Science no Agronegócio.*
