# EDA - Trabalhos e Salários na Área de Dados 🎲

*Autor : Gustavo Costa*

Bem-vindo ao projeto de Análise Exploratória de Dados (EDA) sobre Trabalhos e Salários na Área de Dados! Neste estudo, mergulhamos fundo na vasta quantidade de dados disponíveis para entender as dinâmicas do mercado de trabalho em um dos setores mais dinâmicos e promissores da atualidade: **a área de dados**.

Através de uma análise detalhista, examinei não apenas os salários oferecidos em diferentes funções, mas também as nuances que influenciam esses pacotes salariais. Investiguei a relação entre a experiência profissional e as habilidades especializadas, revelando padrões fascinantes que podem orientar tanto profissionais estabelecidos quanto iniciantes a uma carreira na área de dados.

Este projeto é uma contribuição para a comunidade de dados, e espero que os insights aqui apresentados sejam úteis para todos os interessados em entender melhor o mercado de trabalho na área de dados.

****

## 1. Resumo do Conjunto de Dados.

**Informações das colunas**
 - 'work_year': ano que o dado foi registrado, indica temporalidade, importante
  para entender os salários ao longo do tempo.

 - 'job_title': é o nome específico do cargo, ideal para entender a distribuição salarial entre diversos cargos especializados.

 - 'job_category': é a classificação do cargo em uma categoria, facilitando a análise.

 - 'salary_currency': corresponde ao salário atual recebido pela pessoa que pode ser em diversas moedas diferentes (doláres, euros, etc).

 - 'salary': é o valor recebido em uma determinada moeda.

 - 'salary_in_usd': corresponde ao valor do salário atual padronizado para o dólar, ideal para realizar comparações globais.

 - 'employee_residence': corresponde ao país onde a pessoa reside.

 - 'experience_level': é o nível de experiência da pessoa, interessante para retirar insights relacionados ao salário em relação ao nível de experiência.

 - 'employment_type': é como o trabalho foi acordado: CLT, PJ, Contrato, etc. Ideal para explorar como os diferentes tipos de trabalho afetam a estrutura salarial.

 - 'work_setting': é a configuração do trabalho: home-office, presencial ou hibrido. Também serve para analisar como cada configuração afeta a estrutura dos salários.

 - 'company_location': localização da empresa, pode ajudar também na análise dos salários e como cada localidade interfere no dinheiro.

 - 'company_size':  tamanho da empresa (pequena, média ou grande) também pode afetar os salários de acordo com o seu tamanho.

⚠️ *esse conjunto de dados pode ser encontrado e utilizado aqui: https://www.kaggle.com/datasets/arnabchaki/data-science-salaries-2023* ⚠️

## 2. Análise Salarial em Dólares. 💸
Para iniciar a análise dos salários, foi escolhido realizar a análise em **dólar**, visto que há inúmeros profissionais oriundos de diferentes países ao redor do mundo que recebem em diferentes moedas, logo, para realizar comparativos globais entre os salários foi escolhido a normalização dos salários em USD.

<img src="https://i.ibb.co/tcsLjcD/download-5.png">

A priori, nota-se uma assimetria à direita na distribuição dos salários oque possivelmente influenciará na média salárial devido à altos salários registrados. Somado à isso, a concentração de salários nessa distribuição está na faixa de [100.000 - 200.000] dólares. Com uma descrição dos dados, poderemos visualizar sua média, mediana, etc:

<img src="https://i.ibb.co/gRCcGnV/Captura-de-tela-2024-05-15-145145.png">

Podemos ver que a média salarial global equivale à 150.299,50 dólares anuais aproximadamente. Além disso, temos que a mediana é 143.000,00 dólares anuais, mostrando que a média sofre influência de altos valores. Como há essa influência, precisamos calcular os limites do intervalo interquartil para avaliarmos quais salários são outliers, ou seja, valores que podem enviesar nossa análise, seja valores exorbitantes ou valores muito pequenos. Portanto, criei uma função auxiliar que consiste em apenas calcular os limites IQR para informar quais são os salários considerados como outliers.

<img src="https://i.ibb.co/xsrk2qf/download-6.png">

Veja que, o boxplot da coluna que refere-se aos salários em dólares acusa muitos outliers que são representados pelos círculos. Logo, é mais um motivo para existir a função calculadora de outliers, para que possamos realizar uma **análise separada dos outliers e dos dados sem os outliers**, afinal, nem sempre o outlier deve ser descartado, ele pode carregar informações úteis a depender do contexto. 

<img src="https://i.ibb.co/R6LwLY9/Captura-de-tela-2024-05-15-150440.png">

O uso da função 'calculo_outliers' na coluna dos salários em dólares nos informa que os limites IQR são [-15.834,50 - 308.257,50] dólares. Ademais, calculando a quantidade dos outliers, há 158 pessoas com salários considerados como possíveis outliers. Aqui, vale ressaltar que o limite inferior é negativo, isso diz que só são considerados 'outliers inferiores' aquelas pessoas que recebem **menos que -15.834,50 dólares, oque é totalmente irreal, visto que ninguém recebe salários negativos ou ganha menos que um salário negativo hipotético.**Sendo assim, essas 158 pessoas recebem acima de 308.257,50 dólares anuais.

No primeiro momento, não considerarei tais valores como Outliers, visto que há registros de cargos executivos com salários altissímos, há inúmeras possibilidades das variações salariais, ainda mais se olharmos pelo lado **GLOBAL** : se é CLT ou PJ, moeda do país, o cargo, o tamanho da empresa, etc. Por isso, irei realizar análises salariais por subgrupos.


### 2.1. Análise Salárial por Experiência. ⌛

Utilizando a média, vamos plotar alguns gráficos para visualizar como essa relação entre salários médios e experiências se dá. 

<img src="https://i.ibb.co/TcnJk97/download-7.png">

Por esse gráfico simples de barras, visualmente falando, há uma diferença crescente entre os valores médios salariais conforme o nível de experiência aumenta, o que pode corroborar para um **padrão estar acontecendo**. Mas esse gráfico por si só, não nos comprova nada. Uma forma de realizar essa verificação é compararmos os boxplots de cada nível de experiência e relizarmos **testes de hipótese** que corroboram a idéia de que há uma diferença **significante** entre juniores, plenos, seniores e executivos.

E porque utilizar diferentes boxplots? A resposta é simples: os níveis de experiência são variáveis categóricas e realizar esse procedimento é uma boa forma de podermos compará-las.

<img src="https://i.ibb.co/C55f0c9/download-8.png">

Com esse gráfico, podemos notar claramente o crescimento do salário em relação ao nível de experiência. Um exemplo visível disso, é que **50% dos salários dos sêniores são maiores que 50% dos maiores salários dos plenos.** Agora, resta saber se
a diferença de salário entre diferentes níveis de experiência é, de fato, significativa. Para isso, realizarei testes de hipótese. É importante salientar que os testes de hipótese podem ser feitos porque mesmo cada distribuição por nível de experiência possuindo uma **assimetria à direita**, se utilizarmos as médias de cada distribuição e, assegurarmos que nossa amostra é grande o suficiente, teremos embasamento no **Teorema Central do Limite** para a realização dos testes.

Qual é a nossa hipótese nula?
 - *H0:* A média salarial em dólares dos dois níveis de experiência são iguais:
      
    - junior e pleno;
      
    - junior e senior;
      
    - junior e executivo;

    - pleno e senior;

    - pleno e executivo;

    - senior e executivo.

- *H1:* A média salarial em dólares entre dois níveis de experiência diferentes não são iguais, ou seja, um nível tende a receber maior que o outro em média.

separação de cada nível de experiência.

<img src="https://i.ibb.co/KNk5VB0/Captura-de-tela-2024-05-15-152825.png">

Eu optei por não utilizar o teste Anova, porque por mais que ele diga se há significância entre os 4 níveis, ele não informa onde há essa diferença significante. Portanto, será um pouco mais verboroso realizar 6 testes normais diferentes mas saberemos entre quais pares de níveis de experiência podemos assegurar uma diferença significante, se houver alguma.

*Estou supondo que os desvios-padrão populacionais desconhecidos são diferentes e que o nível de significância é de 5%*.

Se os valores encontrados utilizando a distribuição T forem menores que 0,05 rejeitamos a *H0*.

<img src="https://i.ibb.co/tJdY5Bz/Captura-de-tela-2024-05-15-152846.png">

Os resultados ou p-values encontrados por meio do uso da distribuição t, são tão pequenos que mesmo com a formatação em 5 casas decimais são menores. Logo, todos os p-values são menores que o nível de significância de 0,05.

Resultado: Rejeitamos a *H0* e podemos afirmar que a **média salarial aumenta conforme o nível de experiência.**

### 2.2. Análise Salarial - Juniores. 👶
Análise separada por nível de experiência, primeiramente vamos começar pelos juniores.

<img src="https://i.ibb.co/f99Xfqg/download-9.png">

Podemos observar que há outliers entre os juniores, eles serão analisados separadamente. Para cada subgrupo de nível de experiência terá duas análises: sem outliers e apenas os outliers.

#### Juniores Sem Outliers.

<img src="https://i.ibb.co/F3nn1wj/Captura-de-tela-2024-05-15-154359.png">

Para 488 pessoas registradas como juniores, temos uma média salárial de 85.883,52 dólares anuais e uma mediana de 79.400,00 dólares anuais.

<img src="https://i.ibb.co/FXvNFbp/download-10.png">

Como no dataset há registros de pessoas trabalhando na área de dados a partir de 2020, observa-se nítidamente **um crescimento dos salários médios na área**.

Mas porque há esse crescimento? há possíveis motivos , por exemplo, a constante influência da importância dos dados na manutenção e favorecimento de lucros nas empresas, aceleramento precoce da área de tecnologia pelo escalamento da pandemia durante os anos de 2020/2 - 2021 - 2022.

### Gráfico dos Juniores: Relação Mediana Salarial x Cargo Ocupado

<img src= "https://i.ibb.co/HGsd3g5/download-11.png">

Com a construção gráfica, é possível detalhar a média salárial dos juniores baseado na categoria do seu cargo e na configuração do trabalho (homeoffice, híbrido ou presencial).

- Na questão de ser presencial, a faixa salárial é maior para juniores que atuam na subárea de **Machine Learning e IA, recebendo um salário maior que 120 mil dólares anuais**.

- Já sendo híbrido, o maior valor que está em conformidade com a subárea de **liderança e administração, com uma faixa salarial maior que 120 dólares anuais também.**

- No que se diz respeito aos trabalhos remotos, a subárea de **pesquisa e ciência de dados leva a maior remuneração média anual, por volta de 80 mil dólares.**

Optei pelo uso da mediana na comparação dos cargos e configurações de trabalho porque no dataset **cada categoria não está dividida em registros de mesma quantidade, oque poderia enviesar a média se fosse realizado comparações entre diferentes quantidades**

#### Juniores Outliers.

<img src="https://i.ibb.co/3MKxQtx/Captura-de-tela-2024-05-15-155219.png">

Aos outliers, o único padrão visível entre eles é que todos:
 - residem nos EUA
 - recebem em dólares
 - trabalham em tempo integral

Não há uma similaridade no restante, oque pode explicar essa diferença do restante dos dados são possíveis habilidades incomuns entre esses profissionais, podem estar concentrados em uma área do país que os valorizou. Uma rápida pesquisa no google, observamos que a média via GLASSDOOR é entre 169k-216k de dólares.

<img src="https://i.ibb.co/LJQf4nt/Captura-de-tela-2024-05-15-155342.png">

Supondo que a média salarial da área seja de 216 mil dólares, temos que a *média salarial dos outliers que pertencem ao nível de experiência junior, são de cerca de 15,86% maiores que o comum*.


### 2.3. Análise Salárial - Plenos. 👦

Distribuição dos salários no nível de experiência Pleno.

<img src="https://i.ibb.co/svHQq3B/download-12.png">


#### Plenos Sem Outliers.

<img src="https://i.ibb.co/yXJ8nCn/Captura-de-tela-2024-05-15-155831.png">

Temos que, para os plenos, a média salárial anual é de 114.197,68 dólares. Já a mediana é de 109.000,00 dólares anuais.

<img src="https://i.ibb.co/L6fFCgB/download-13.png">

Pelo gráfico simples de linha, é nítido o aumento da média salárial anual dos plenos ao longo dos anos. Mas vamos verificar essa informação pelo teste de hipótese.

<img src="https://i.ibb.co/6YcwtSk/download-14.png">

- *H0:* As médias dos salários de plenos são iguais para todos os anos de registro

- *Hipótese Alternativa:* As médias salariais para plenos aumentaram ao longo dos anos.

*Significância de 5% e desvios-padrão diferentes e desconhecidos*

<img src="https://i.ibb.co/ZXtzfPM/Captura-de-tela-2024-05-15-160622.png">

Os resultados, em sua respectiva ordem nos trás, estatisticamente falando, as seguintes informações:

  - O p-value é maior que 0,05. Logo, não rejeitamos a hipótese nula de que as médias entre os anos de 2020 e 2021 são iguais.

  - O p-value é menor que 0,05. Logo, rejeitamos a hipótese nula de que as médias entre os anos de 2020 e 2021 são iguais e aceitamos a hipótese alternativa de que houve um aumento na média salárial entre os anos de  2022 e 2023.

  - O p-value é menor que 0,05. Logo, rejeitamos a hipótese nula de que as médias entre os anos de 2020 e 2021 são iguais e aceitamos a hipótese alternativa de que houve um aumento na média salárial entre os anos de 2020 e 2022.

Assim, **podemos afirmar com embasamento no teste T que a média salárial no nível pleno aumentou conforme os anos de registro.**

#### **Gráfico dos Plenos: Relação Mediana Salarial x Cargo Ocupado**

<img src="https://i.ibb.co/my1W3Nf/download-15.png">

Com a construção gráfica, é possível detalhar a média salárial dos plenos baseado na categoria do seu cargo e na configuração do trabalho (homeoffice, híbrido ou presencial).

- Na questão de ser presencial, a faixa salárial é maior para plenos que atuam na subárea de **Machine Learning e IA, recebendo um salário maior que 140 mil dólares anuais**.

- Já sendo híbrido, o maior valor que está em conformidade com a subárea de **Machine Learning e IA também, com uma faixa salarial maior que 100 mil dólares anuais.**

- No que se diz respeito aos trabalhos remotos, a subárea de **Modelagem e Arquitetura de Dados leva a maior remuneração média anual, ultrapassando os 140 mil dólares.**

#### Plenos Outliers.

<img src="https://i.ibb.co/zXMYqws/Captura-de-tela-2024-05-15-161318.png">

<img src="https://i.ibb.co/Khpbx57/Captura-de-tela-2024-05-15-161414.png">

Aqui existem 6 pessoas dentre as 30 consideradas como Outliers que recebem 300k-350k de **libras esterlinas**, que durante a análise global em dólares são convertidos para salários exorbitantes, principalmente para plenos.

Uma rápida pesquisa no google, nos traz uma perspectiva que no nível pleno (mid-level) recebem na faixa de 64k-102k libras esterlinas. Logo, esses salários são altíssimos em comparação com a média do país.

Após uma consulta em sites como Linkedin, Glassdoor, Indeed, a faixa salarial para os EUA no mid-level (pleno) é na faixa de 132k-210k de dólares.

Se aprofundarmos nessa análise, esses salários altos correspondem à empresas multinacionais como a Meta, detentora de redes sociais, Google, etc.

Para tal análise, vamos realizar uma correlação entre essas duas variáveis. Como o tamanho da empresa é uma var. qualitativa, vamos considerar o seguinte:

  - 1: empresas pequenas / small companies
  - 2: empresas médias / medium companies
  - 3: empresas grandes / large companies

<img src="https://i.ibb.co/DKK6Yhf/Captura-de-tela-2024-05-15-161540.png">

<img src="https://i.ibb.co/8Yqvxbs/Captura-de-tela-2024-05-15-161619.png">

A correlação é **fraca e positiva**, isso indica que há uma baixa tendência de, à medida que o tamanho da empresa aumenta, o salário tende a aumentar um pouco. Entretanto, outros fatores como: habilidades específicas, tempo de experiência no cargo, etc influenciam esses valores altos dos salários e só o tamanho da empresa **não é forte o suficiente para assegurar essa tendência**.

### 2.4. Análise Salarial - Sêniores. 🧔

<img src="https://i.ibb.co/SXhm7rX/download-16.png">

O boxplot dos salários em dólares dos seniores acusa muitos outliers.

<img src="https://i.ibb.co/pbxGfJH/Captura-de-tela-2024-05-15-162054.png">

Outliers no nível Senior são aqueles cujo valores extrapolam 313.100 dólares anuais.

#### Sêniores sem Outliers.

<img src="https://i.ibb.co/nc841Fn/download-17.png">

Visualmente falando, a tendência é que a média salarial dos seniores que não fazem parte dos Outliers também cresce consideravelmente ao longo dos anos. Para confirmar essa informação, é possível realizar os testes de hipótese.

<img src="https://i.ibb.co/nRzXQSB/download-18.png">

**Teste de hipótese seniores**

- *H0:* As médias salariais para os seniores não aumentam ao longo dos anos de registro.

- *Hipótese Alternativa:* As médias salariais aumentam conforme os anos passam.

*Obs: nível de significância de 5% e desvios-padrão diferentes desconhecidos*

Três testes serão realizados:

- se a média salarial de 2021 é maior que a de 2020;

- se a média salarial de 2022 é maior que a de 2021;

- se a média salarial de 2023 é maior que a de 2022;

Caso todas sejam verdadeiras, isso implica que a média salarial aumentou ao passar dos anos.

<img src="https://i.ibb.co/y8w8qx0/Captura-de-tela-2024-05-15-162400.png">

Os resultados confirmam a nossa hipótese nula (H0) já que para todos os testes feitos, o valor está acima do p-value de 0,05.

Logo, isso corrobora a ideia de que a **média salarial dos seniores aumentou durante os anos de registro.**

#### Sêniores Outliers.

O menor salário de um sênior considerado como Outlier pelos limites IQR é de 315.000 dólares. Já o maior é de 412.00 dólares.

Dentre 106 indivíduos considerados Outliers, há apenas 2 que não recebem em dólar.

Nesses países, de acordo com o Glassdoor, a faixa salarial em grandes empresas como Amazon, Facebook, Microsoft é por volta de 120k de euros. O que evidencia como é muito discrepante os salários dessas pessoas, confirmando que são outliers.

#### **Gráfico dos Sêniores: Relação Mediana Salarial x Cargo Ocupado**

<img src="https://i.ibb.co/K5VtBBR/download-19.png">

Com a construção gráfica, é possível detalhar a média salárial dos sêniores baseado na categoria do seu cargo e na configuração do trabalho (homeoffice, híbrido ou presencial).

- Na questão de ser presencial, a faixa salárial é maior para sêniores que atuam na subárea de **Machine Learning e IA, recebendo um salário maior que 175 mil dólares anuais**.

- Já sendo híbrido, o maior valor que está em conformidade com a subárea de **Arquitetura e Modelagem de Dados, com uma faixa salarial maior que 150 mil dólares anuais.**

- No que se diz respeito aos trabalhos remotos, a subárea de **Machine Learning e IA leva a maior remuneração média anual, ultrapassando os 175 mil dólares.**

### 2.5. Análise Salárial - Executivos. 🤵

Há 281 pessoas consideradas como executivas na área de dados.

<img src="https://i.ibb.co/nMNCrjS/download-20.png">

Há apenas 1 outlier encontrado para os executivos. Os limites IQR são [-2500,00 - 377.500,00]

#### Executivos sem Outliers.

<img src="https://i.ibb.co/rw3MHph/download-21.png">

Visualmente falando, vemos uma queda acentuada de 2020 para o ano de 2021, oque pode ter sido influenciado pela pandemia do Covid-19 que trouxe falência à empresas, desemprego e redução salarial. Entretanto, de 2021 para 2022 houve um salto grande, com um aumento de  mais de 30 mil dólares nos salários médios dos executivos da área da ciência de dados.

<img src="https://i.ibb.co/xDYC2Dn/download-22.png">

É nítido que o **número de contratações de executivos na área de dados foi muito maior no ano de 2020** e também observa-se a variação entre as medianas salariais para cada boxplot. Essa diferença de contratações também influencia na média salarial, afinal a média é a soma dos salários divididos pela quantidade de pessoas.

#### Executivo Outlier.

O único executivo considerado como outlier pelo cálculo IQR é o Cientista de Dados Principal, tem um regime de contrato com a empresa e reside nos Estados Unidos da América, recebendo 416 mil dólares anuais.


#### **Gráfico dos Executivos: Relação Mediana Salarial x Cargo Ocupado**

<img src="https://i.ibb.co/gJYQh9H/download-23.png">

Com a construção gráfica, é possível detalhar a média salárial dos executivos baseado na categoria do seu cargo e na configuração do trabalho (homeoffice, híbrido ou presencial).

- Na questão de ser presencial, a faixa salárial é maior para executivos que atuam na subárea de **Machine Learning e IA, recebendo um salário maior que 250 mil dólares anuais**.

- Já sendo híbrido, o maior valor que está em conformidade com a subárea de **Liderança e Gerência, com uma faixa salarial maior que 150 mil dólares anuais.**

- No que se diz respeito aos trabalhos remotos, a subárea de **Engenharia de Dados leva a maior remuneração média anual, ultrapassando os 200 mil dólares.**


### Extra: Mapa Mundi - Salário Médio em Dólares & Localidade 🌍

<img src="https://i.ibb.co/cykJbK4/newplot.png">
