# Documentação - mapas interativos
Este documento apresenta um guia de uso dos scripts desenvolvidos para a plotagem dos mapas envolvendo a taxa de mobilidade e os indicadores epidemiológicos relacionados ao período que vai de fevereiro a agosto de 2020.  Esse recorte corresponde aos primeiros meses da chegada da pandemia de Covid-19 no Brasil. Tanto esses códigos quanto os dados utilizados para gerar os mapas são fruto do trabalho desenvolvido pela equipe do projeto Covid Data Analitycs (CDA), coordenado pelo professor Wagner Meira Júnior, do Departamento de Ciência da Computação da UFMG. Os resultados do projeto e mais informações podem ser encontradas no nosso [portal](http://covid.dcc.ufmg.br/). 

## Instruções de uso detalhadas 
### 1- Preparando o ambiente
Antes de tudo, é preciso preparar a máquina para que os scripts possam ser rodados. Eles foram desenvolvidos no sistema operacional Windows 10 com a linguagem de programação Python com o auxílio do ambiente de programação Jupyter Notebook. A versão do python utilizada foi a 3.7.6, logo é provável que surjam erros inesperados caso eles sejam rodados em outras versões.

Como a aplicação utiliza não somente os módulos básicos do Python, mas também muitas das suas bibliotecas (que expandem suas funções), recomendamos que seja feita a instalação da distribuição Anaconda, que já contém o Python e muitos módulos irão ser necessários. Essa distribuição pode ser baixada gratuitamente por meio [deste link](https://www.anaconda.com/products/individual).

Algumas bibliotecas adicionais também devem ser instaladas após a instalação do Anaconda. Para isso, basta digitar “cmd” na barra de pesquisa do windows e clicar em “Prompt de Comando”. Dentro do prompt, digite os seguintes comandos para a instalação de bibliotecas necessárias, pressionando enter depois de cada um deles:
    
    pip install folium
    pip install geopandas
    pip install geobr

É esperado que as outras bibliotecas necessárias já estejam contidas dentro da distribuição Anaconda, então não será necessário instalá-las novamente. Porém, caso durante a execução surja algum erro relacionado à falta de alguma delas, basta pesquisar no [repositório oficial de bibliotecas do Python](https://pypi.org/) o comando necessário para sua instalação.

### 2 - Obtendo os arquivos necessários
Os scripts desenvolvidos para a plotagem dos mapas estão nomeados como “Mobilidade_mapas.ipynb” e “Indicadores_mapas.ipynb” e podem ser encontrados no [repositório do CDA no GitHub](https://github.com/CDA-EPCWeb/Visual).Eles estão Contidos dentro da pasta “Mapas Interativos”.

Além desses arquivos, também será preciso obter de antemão as bases de dados dos indicadores epidemiológicos e a que contém os dados dos municípios brasileiros, denominadas respectivamente como “indicadores.db” e “municípios.csv”. Elas são disponibilizadas pelo CDA [neste link](https://drive.google.com/drive/folders/1pJOwnLmJAGMj6gmi_16QcSQ7Lkh3JoN_?usp=sharing). 

Outra base de dados necessária, relativa aos dados de mobilidade, é a contida no arquivo “2020_BR_Region_Mobility_Report.csv”, que pode ser baixado clicando no botão “CSVs por região” disponível [neste link](https://www.google.com/covid19/mobility/index.html?hl=pt-BR). Esses dados são disponibilizados pelo Google e mais informações sobre como eles foram coletados podem ser encontradas [aqui](https://support.google.com/covid19-mobility/answer/9824897?hl=pt-BR&ref_topic=9822927).

A plotagem dos mapas está dividida entre os arquivos “Mobilidade_mapas.ipynb” e “Indicadores_mapas.ipynb” porque o primeiro é responsável pela plotagem dos indicadores relacionados à mobilidade das pessoas entre diversas categorias de locais ao longo do tempo. Já o segundo plota a visualização geoespacial dos indicadores epidemiológicos relacionados diretamente com a pandemia ao longo do tempo, tais como número de óbitos e incidência.

### 3 - Utilizando os scripts
Escolha uma pasta de seu computador e coloque nela todos os arquivos necessários. Após isso copie o endereço da pasta no computador, localizado na parte superior do explorador de arquivos do Windows. 

![pasta](https://user-images.githubusercontent.com/62709699/100187387-c3e68980-2ec6-11eb-9f24-e7a300ff2869.JPG)

Digite “cmd” na barra de pesquisas e abra o Prompt de Comando. Nele, digite o comando “cd” e logo a frente dê um espaço e cole o endereço da pasta onde estão os arquivos e pressione enter. Agora o prompt de comando pode executar comandos dentro dessa pasta. Então digite simplesmente “jupyter notebook” e aguarde uma aba do seu navegador padrão abrir, mostrando os diversos arquivos da pasta.

![cmd](https://user-images.githubusercontent.com/62709699/100187434-dd87d100-2ec6-11eb-9bf8-60d73f7e3435.png) 

Essa é a interface inicial do Jupyter Notebook, o ambiente de programação que será usado para a plotagem dos mapas. Com ele é possível abrir os arquivos “Mobilidade_mapas.ipynb” e “Indicadores_mapas.ipynb” para que possam ser modificados ou executados. Vamos começar com o primeiro deles, explicando o passo-a-passo até que o mapa possa ser gerado.

![jupyter](https://user-images.githubusercontent.com/62709699/100187492-fabc9f80-2ec6-11eb-9d3e-0c848605a08e.png)

### 4 - Mapas de mobilidade 
Abra o arquivo “Mobilidade_mapas.ipynb” e observe que o código é dividido em células que podem ser executadas separadamente, que estão agrupadas em seções com diferentes objetivos. A primeira célula contém as importações de bibliotecas adicionais que serão necessárias para a execução do código. A execução dessa célula não deve apresentar nenhum erro, caso contrário isso significa que alguma das bibliotecas necessárias ainda não foi instalada.

Na **Seção 1**, “Obtendo os dados geográficos”, os dados geográficos oficiais do Brasil são importados da biblioteca GeoBR. Eles serão usados posteriormente para delimitar os limites dos estados brasileiros no mapa. O pacote GeoBR foi criado por uma equipe de desenvolvedores no Instituto de Pesquisa Econômica Aplicada (Ipea) e mais informações sobre ele podem ser encontradas neste link.

Na **Seção 2**, “obtendo e adequando dados de mobilidade”, os dados de mobilidade são importados e moldados para que restem somente aqueles que são necessários para a plotagem dos mapas, sendo eliminados aqueles que são desnecessários. A partir da importação dos dados que vamos utilizar, eles passarão a estar contidos no dataframe “mobility_df”. Os dataframes são objetos disponibilizados pela biblioteca Pandas que funcionam como tabelas, mas que podem ser manipuladas pelo python para realizarmos as mais diversas operações.
	
Primeiramente são mantidos somente os dados relativos aos estados como um todo, já que também estão presentes na base de dados informações relativas a outras dimensões geográficas (como os dados das cidades, por exemplo). 

Logo depois, o formato das datas é corrigido, para que elas fiquem mais fáceis de serem manipuladas. Assim, logo em seguida mantemos somente os dados de um intervalo de tempo específico (precisa ser um intervalo que está contido dentro da base de dados).

Na **Seção 3**, “Moldando dados para plotagem”, o conjunto de dados que temos é rearranjado de uma forma que ele seja compreendido pela biblioteca que realiza a plotagem dos mapas. Nessa biblioteca, chamada Folium, é feito o uso de um plugin específico chamado “TimeSliderChoropleth”, que é capaz de criar um mapa interativo a partir de um dataframe com os limites de uma ou várias áreas geográficas e um dicionário contendo o nome dessas áreas, uma medida de tempo e uma medida de cor (em hexadecimal). Com ele é possível criar um mapa coroplético com cores representando determinado dado e que podem ser alteradas pelo usuário de acordo com a seleção de uma data específica no controle deslizante disponibilizado na parte superior.

O dataframe com os limites geográficos dos estados é aquele que foi obtido na Seção 1, a partir da biblioteca GeoBR. Porém, ainda é preciso criar o dicionário usado pelo plugin. Esse dicionário precisa ter exatamente a seguinte estrutura:    
###### 'Nome_da_area': {'Tempo_em_milissegundos': {'color': 'cor_em_hexadecimal', 'opacity': ‘opacidade’}
.
Um exemplo de trecho de um dicionário desse tipo é destacado a seguir:

    
    {'DF': {'1582416000': {'color': '#fff2d1', 'opacity': 0.7},
    '1582502400': {'color': '#fdae61', 'opacity': 0.7},
    '1582588800': {'color': '#fdae61', 'opacity': 0.7},
    …}
    
Observe que dos diversos valores que precisamos colocar no dicionário, somente a opacidade da cor é fixa. Já os valores de tempo e cor precisam ser calculados a partir do dataframe resultante da Seção 2. 

Nessa seção 3, inicialmente é realizada a transformação do nome das colunas por uma questão de praticidade, já que os nomes originais são muito extensos. Em seguida a coluna de datas tem seus valores alterados para que essas elas sejam representadas no formato de milissegundos, que é o formato que o plugin exige.

Porém, ainda é necessário calcular os valores de cor e criar o dicionário que precisaremos na plotagem. Para isso, foi criada a função “criarDict”. Nela, primeiro os dados são divididos em um espectro baseado no menor e maior valores disponíveis para o indicador. Depois são associadas cores previamente estabelecidas a essas faixas de dados. Dessa forma, por exemplo, caso seja observado que o estado de Minas Gerais teve uma mobilidade baixa em Mercados e Farmácias durante determinada data, o mapa correspondente a esse dado vai ter uma cor mais azulada sobre a área desse estado nessa data.

Na última parte dessa mesma função os dados são organizados no formato solicitado (que foi mostrado aqui anteriormente). O Resultado é um dicionário de dicionários onde as chaves representam o indicador que se quer plotar e que será usado mais adiante.

Na **Seção 4** é onde finalmente plotamos os mapas. Para isso utilizamos a biblioteca do Folium que já citamos aqui chamada “TimeSliderChoropleth”. Uma parte muito importante que é preciso observar é a de que a variável “categoria” precisa ter o seu valor alterado (dentre aqueles disponíveis) para que diferentes mapas de mobilidade possam ser plotados. O título do mapa e o título da legenda contida nele também devem ser alterados manualmente de acordo com o indicador de mobilidade escolhido.

Observe também que a amplitude da escala da legenda é definida automaticamente, sendo definida de acordo com o maior e o menor valores disponíveis para cada indicador. 

No código, a função “folium.Map()” define a base do mapa, estabelecendo parâmetros como a localização e o zoom da visualização que o usuário terá ao abrir o mapa. Em seguida, a função “TimeSliderChoropleth()” plota a camada que contém a informação visual que buscamos mostrar. Observe que ela recebe como argumentos o dataframe que contém o formato dos estados, criado na Seção 1 e o dicionário que foi criado na Seção 3.

A função “folium.GeoJson()” funciona nesse caso somente para desenhar o limite entre os estados, já a “cm.LinearColormap()” é a responsável por plotar a legenda no canto superior direito do mapa. Por fim, “mapa.save()” salva o mapa plotado no mesmo local onde o código foi executado em seu computador. O arquivo será salvo no formato HTML, que pode ser aberto em qualquer navegador de internet.

### Mapas de indicadores epidemiológicos
Na interface inicial do Jupyter Notebook, abra o arquivo “Indicadores_mapas.ipynb”, que está separado em seções. A primeira célula contém as importações de bibliotecas adicionais que serão necessárias para a execução do código. A execução dessa célula não deve apresentar nenhum erro, caso contrário isso significa que alguma das bibliotecas necessárias ainda não foi instalada.Caso tenha ocorrido um erro digite *pip install* e o nome da biblioteca, rode a cédula novamente para que a biblioteca que está faltando seja instalada.

Na seção **“Definindo o indicador que será plotado entre”**, definimos com qual dos 6 indicadores iremos trabalhar ao longo desse notebook. A disposição temos os indicadores new_week_cases, new_week_deaths, incidence_cases, incidence_deaths, cases_growth_factor, deaths_growth_factor, cada um com a sua descrição já no código. Para alternar entre os indicadores basta mudar o nome dentro da variável *indicador*, que o script vai tratar do resto das alterações.

Na seção **“Seleciona em 'indicadores.db' apenas o df 'cities'”** e nas três seguintes, é selecionado da database o data frame com o qual iremos trabalhar ao longo do notebook, sendo que as opções são “brazil_df”, “cities_df”, “regions_df” e “states_df”. É nesse momento também que captamos do arquivo csv 'municipios.csv' atributos como ('codigo_ibge', 'latitude', 'longitude') que serão importantes para localizar os dados geograficamente em um momento posterior (já que eles definirão o local onde cada ponto do mapa será plotado). 

Após ter esses dois data frames, é realizado um merge (junção) visando adicionar os dados de latitude e longitude de cada município em 'cities_df'. Dessa forma, podemos ter na mesma base de dados, as informações importantes de cada uma das duas anteriores. 

Em **“Formatando data”**, é feita uma pequena correção no formato das datas desse novo dataframe para que elas sejam armazenadas em um formato reconhecível durante a plotagem dos mapas.

Na seção **“Drop nas colunas extras”**, após tratar a formatação da data podemos remover do data frame as colunas que não serão utilizadas para o plot do mapa. 

Na Seção **“Fronteiras dos estados”** usamos o dataframe “shape_br”, que contém os dados geográficos de limites territoriais, para desenhar as fronteiras dos estados sobre o mapa que o folium fornece. Existe a opção de personalizar as características das linhas das fronteiras alterando os atributos descritos no código. 

A seguir são removidas do dataframe as linhas em que constam zero casos, visando facilitar a visualização e auxiliar na plotagem de um mapa mais leve.

Nas partes **“Definindo escala”** e **“Definindo cores”**, primeiro são definidos números arbitrários, baseados em testes, qual escala será seguida por cada indicador plotado para que a visualização se torne mais interessante para o usuário. Logo depois são definidas as cores de plotagem dos círculos que representam os indicadores no mapa.

Uma das partes mais importantes é a denominada **“Convertendo dados para json”**, na qual todos os dados que foram trabalhados até esse ponto do código por meio de dataframes são convertidos para um formato chamado “geojson”. Isso é necessário já que o plugin utilizado para a plotagem dos dados só recebe esse tipo de entrada.
Finalmente temos a seção **“Plotando com o Folium”**, onde o plugin TimestampedGeoJson da biblioteca Folium é utilizado para a plotagem do mapa interativo usando como base o geojson criado anteriormente (denominado “start_geojson”). Observe que a última linha define que o mapa gerado será salvo no formato html com o mesmo nome do indicador epidemiológico escolhido. Sendo assim, se tudo ocorrer bem, após a execução do código, surgirá na mesma pasta do script esse arquivo html.
