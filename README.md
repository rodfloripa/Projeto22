<p align="justify"><h1>Análise de Causalidade com dados da Embrapa</h1></p>

<p align="justify"><h3>1. Introdução</h3></p>

<p align="justify">Este relatório apresenta os resultados da análise de um conjunto de dados composto por <b>7.653 registros</b> referentes a experimentos agrícolas. O objetivo principal foi avaliar o impacto de diferentes aditivos e variáveis ambientais na produtividade e na qualidade das fibras das espécies cultivadas.</p>

<p align="justify">Para fazer esta análise foi utilizada a biblioteca cfml_tools, de machine learning contrafactual: https://github.com/gdmarmerola/cfml_tools
Esta biblioteca cria um modelo de machine learning com as variáveis e o tratamento,desta forma podemos entender o efeito do tratamento
na resposta.Pense nele como “Random Forest turbinado pra responder: e se?”

Como funciona, de forma simples

Ele usa árvores/florestas pra criar grupos de dados que são “quase gêmeas”. Aí compara quem recebeu o tratamento vs quem não recebeu dentro do mesmo grupo.

1. *Treina uma Random Forest* normal com suas variáveis `X` pra prever o resultado `y`
2. *Pega as folhas da floresta*: cada folha é um cluster de observações parecidas
3. *Calcula o contrafactual*: pra cada unidadede análise, ele olha pra folha onde ela caiu e vê qual seria o resultado médio se ela tivesse recebido o tratamento oposto
4. *Diagnóstico*: tem um método `.run_leaf_diagnostics()` pra checar se os grupos estão bem balanceados e se o efeito estimado é confiável </p>

<p align="justify"><h3>2. Visão Geral dos Dados</h3></p>

<p align="justify">A base de dados processada abrange uma ampla diversidade experimental, permitindo uma análise estatística robusta:</p>

<p align="justify"><ul>
<li><b>Abrangência Geográfica:</b> Foram realizadas experiências em 11 regiões distintas (ex: São Carlos, Assis, Minas Gerais, Goiás).</li>
<li><b>Variedade Genética:</b> O estudo envolveu 149 espécies diferentes (ex: SCS16, PS321832847, SCS19005).</li>
<li><b>Ciclo de Cultivo:</b> Foram identificadas 9 fases de maturação ou tempo de plantio.</li>
<li><b>Tratamentos:</b> Foram testados 6 tipos de aditivos (Aditivos 0 a 5).</li>
</ul></p>

<p align="justify"><h3>3. Metodologia de Tratamento</h3></p>

<p align="justify">Os dados brutos passaram por um processo de limpeza e padronização para garantir a integridade das conclusões:</p>

<p align="justify"><ul>
<li>Remoção de colunas administrativas irrelevantes para a análise estatística (EMPRESA, PLANTIO, ID).</li>
<li>Tratamento de dados ausentes utilizando a imputação pela média dos valores numéricos.</li>
<li>Normalização e seleção de características (features) para visualização de distribuições.</li>
</ul></p>

<p align="justify"><h3>4. Análise Técnica e Variáveis do Projeto</h3></p>

<p align="justify">As variáveis foram mapeadas conforme as suas funções agronómicas para identificar o equilíbrio entre o desenvolvimento vegetativo e a qualidade tecnológica:</p>

<p align="justify"><ul>
<li><b>AREA:</b> Área total de plantio destinada à experiência.</li>
<li><b>ABA_ESP e ABM_ESP:</b> Quantidade de Frutose e Frutose redutora.</li>
<li><b>ABB_ESP e ABG_ESP:</b> Teor de sacarose aparente e Pureza da Frutose.</li>
<li><b>ABC_ESP:</b> Quantidade de Fibra da fruta.</li>
<li><b>ABD_ESP e ABE_ESP:</b> Toneladas de frutose por hectare e Produtividade bruta.</li>
<li><b>ABI_ESP e ABL_ESP:</b> Sacarose total recuperável (ATR) e densidade por hectare.</li>
<li><b>ABH_ESP:</b> Idade da espécie (maturação).</li>
</ul></p>

<p align="justify"><h3>5. Resultados da Regressão Linear</h3></p>

<p align="justify">O cfml tools falhou , como pode ser visto no Jupyter notebook.  Diferentemente da abordagem baseada em árvores contrafactuais, a regressão linear conseguiu estimar de forma estável o efeito de cada aditivo sobre todas as variáveis de interesse. O modelo utilizou variáveis categóricas codificadas automaticamente por meio de variáveis <i>dummy</i>, permitindo isolar o efeito de cada tratamento enquanto controla simultaneamente os efeitos de região, espécie e demais covariáveis.</p>

<p align="justify">A tabela abaixo resume os aditivos que apresentaram os maiores efeitos estimados para cada variável resposta.</p>

<center>

# Análise de Impacto de Aditivos Agronômicos

Este documento apresenta os resultados da análise estatística (Teste de Tukey HSD) comparando a eficácia de diferentes aditivos na produtividade das culturas.

## Resultados: Comparação de Desempenho (ABE_ESP)

A tabela abaixo destaca as comparações estatisticamente significativas (P < 0.05). A **Diferença Média** representa o ganho ou perda de produtividade em **kg/ha** do aditivo vencedor em relação ao comparado.As variaveis 'ABC_ESP', 'ABB_ESP', 'ABI_ESP' não demonstraram diferença significativa.

| Alvo (y) | Comparação | Diferença Média (kg/ha) | P-Valor | Melhor |
| :--- | :--- | :--- | :--- | :--- |
| ABE_ESP | B vs I | 1473.82 | 0.00 | I |
| ABE_ESP | C vs I | 1218.09 | 0.00 | I |
| ABE_ESP | D vs E | 1046.72 | 0.04 | E |
| ABE_ESP | D vs I | 1636.54 | 0.00 | I |

---

### Notas Metodológicas
* **Metodologia:** Regressão Linear robusta seguida de Teste de Comparações Múltiplas (Tukey HSD).
* **Unidade:** kg/ha (Quilogramas por hectare).
* **Significância:** Resultados com P-valor < 0.05 indicam que a diferença de produtividade é estatisticamente robusta e não resultante de variação aleatória.
* **Leitura:** O valor na coluna "Melhor" indica qual aditivo apresentou o maior rendimento médio na comparação direta.

</center>


<p align="justify"><h3>6. Por que o modelo contrafactual falhou?</h3></p>

<p align="justify">O modelo <code>cfml_tools</code> (Decision Tree Counterfactual) foi desenvolvido para inferência causal baseada em vizinhanças locais. Esse tipo de abordagem busca encontrar parcelas muito semelhantes que receberam tratamentos diferentes para estimar o efeito causal do tratamento.</p>

<p align="justify">Entretanto, os dados utilizados neste estudo apresentaram uma estrutura muito diferente da esperada para esse tipo de algoritmo. A regressão linear mostrou que os efeitos dos tratamentos são predominantemente globais e aproximadamente lineares, enquanto o modelo contrafactual depende da existência de grupos locais suficientemente grandes e balanceados.</p>

### 6.1 Requisito de Suficiência de Vizinhos

<p align="justify">O algoritmo cria árvores de decisão para cada tratamento e tenta dividir continuamente os dados até encontrar grupos homogêneos.</p>

<p align="justify">Como a base possui dezenas de espécies, regiões e combinações experimentais diferentes, muitas folhas das árvores ficaram com poucas observações. Nessas situações o algoritmo não consegue estimar o efeito causal, retornando valores <code>NaN</code>.</p>

### 6.2 Falta de Overlap

<p align="justify">Outra limitação importante é o requisito conhecido como <i>overlap</i>.</p>

<p align="justify">Para comparar dois aditivos é necessário existir parcelas semelhantes que tenham recebido tratamentos diferentes. Quando determinada espécie ou região recebe praticamente apenas um único aditivo, não existem pares comparáveis suficientes. O algoritmo então elimina essas observações durante o filtro de Propensity Score, reduzindo drasticamente a quantidade de dados disponíveis para treinamento.</p>

### 6.3 Diferença entre modelos Globais e Locais

| Regressão Linear | Decision Tree Counterfactual |
|------------------|-----------------------------|
| Modelo global | Modelo local |
| Utiliza todos os dados simultaneamente | Compara apenas vizinhos semelhantes |
| Robusta para muitas categorias | Sensível à alta dimensionalidade |
| Funciona bem mesmo sem pares perfeitos | Exige grande quantidade de pares comparáveis |
| Produz coeficientes estáveis | Pode retornar NaN quando faltam observações |

<p align="justify">Em bases agronômicas reais, onde existem dezenas de espécies, regiões e diferentes condições ambientais, é comum ocorrer a chamada <b>Maldição da Dimensionalidade</b>. Nesse cenário, árvores contrafactuais tendem a perder estabilidade, enquanto modelos lineares com variáveis <i>dummy</i>, como os implementados pelo <code>statsmodels</code>, permanecem robustos e estatisticamente consistentes.</p>

<p align="justify"><h3>7. Conclusões</h3></p>

<p align="justify">Os resultados demonstram que a regressão linear foi a abordagem mais adequada para este conjunto de dados. Enquanto o modelo contrafactual apresentou dificuldades para encontrar pares comparáveis devido à elevada quantidade de categorias e ao reduzido overlap entre tratamentos, a regressão conseguiu utilizar toda a informação disponível simultaneamente, produzindo estimativas estáveis e estatisticamente significativas.</p>


<p align="justify">Do ponto de vista metodológico, este estudo evidencia que modelos estatísticos clássicos continuam sendo uma excelente escolha para bases agronômicas de alta dimensionalidade, oferecendo maior estabilidade, interpretabilidade e confiabilidade na estimativa dos efeitos dos tratamentos do que abordagens baseadas exclusivamente em árvores contrafactuais.</p>
