<p align="justify"><h1>Análise de Causalidade com dados da Embrapa</h1></p>

<p align="justify"><h3>1. Introdução</h3></p>

<p align="justify">Este relatório apresenta os resultados da análise de um conjunto de dados composto por <b>7.653 registros</b> referentes a experimentos agrícolas. O objetivo principal foi avaliar o impacto de diferentes aditivos e variáveis ambientais na produtividade e na qualidade das fibras das espécies cultivadas.</p>

<p align="justify">Para fazer esta análise foi utilizada a biblioteca cfml_tools, de machine learning contrafactual: https://github.com/gdmarmerola/cfml_tools
Esta biblioteca cria um modelo de machine learning com a variável independente,dependente e o tratamento,desta forma podemos entender o efeito do tratamento
na resposta.</p>

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

<p align="justify"><h3>5. Matriz de Desempenho dos Aditivos (Ajustada)</h3></p>

<p align="justify">A tabela abaixo resume a performance comparativa real extraída das médias preditas no notebook, identificando os líderes em cada categoria crítica:</p>

<center>

| Variável Crítica | Descrição Técnica | Melhor Aditivo | Pior Aditivo |
| --- | --- | --- | --- |
| **ABE_ESP** | Produtividade Bruta | **Aditivo 0** | Aditivo 1 |
| **ABC_ESP** | Quantidade de Fibra | **Aditivo 1** | - |
| **ABB_ESP** | Teor de Sacarose | **Aditivo 2** | - |
| **ABI_ESP** | Sacarose Total Recuperável | **Aditivo 2** | - |

</center>

<p align="justify"><h3>6. Conclusões e Recomendações Técnicas</h3></p>

<p align="justify">A análise dos dados confirma um <i>trade-off</i> fundamental entre volume e qualidade. O <b>Aditivo 0</b> é o mais indicado para maximizar a produtividade bruta (volume total), enquanto o <b>Aditivo 2</b> apresenta a melhor performance tecnológica para a extração de açúcar (sacarose aparente e recuperável).</p>

<p align="justify">Em contraste, o <b>Aditivo 1</b>, apesar de ser o melhor para a <b>Quantidade de Fibra (ABC_ESP)</b>, é o que apresenta o pior desempenho em produtividade, reduzindo drasticamente o volume colhido. Portanto, o uso do Aditivo 1 deve ser estritamente limitado a cultivos onde a fibra é o subproduto de maior interesse comercial. Para otimização de rentabilidade industrial por hectare, o foco deve recair sobre os <b>Aditivos 0 e 2</b>.</p>
