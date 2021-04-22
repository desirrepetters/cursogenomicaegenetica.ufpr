---
title: "Aula 02 - Avaliação e processamento de dados brutos de sequenciamento de genomas e transcriptoma (Illumina)"
linkTitle: "Aula 02 - Avaliação e processamento de dados brutos de sequenciamento de genomas e transcriptoma (Illumina)"
weight: 2
description: >
  Softwares utilizados: FastQC, Trimmomatic e Galaxy (online).
---
<div align="justify">
Após o sequenciamento de uma linhagem de interesse para um projeto, são obtidos arquivos com dados brutos que servem de base para a montagem de genomas e transcriptomas e análises futuras. Nesse momento, é extremamente importante avaliar os dados brutos com muita atenção.
<br><br>
Muitas das perguntas de um estudo são respondidas e muitas hipóteses são testadas com base na informação existente nas sequências de um genoma ou um transcriptoma. Além disso, diversas ferramentas de anotação utilizam as características presentes em uma sequência para realizar predições. Dessa forma, caso as sequências estejam incorretas, o impacto negativo nos resultados e conclusões pode ser muito sério.
<br><br>
Levando estas questões em consideração, percebemos que simplesmente montar e utilizar todos os reads brutos que saem do equipamento sem qualquer tipo de controle de qualidade não é uma boa estratégia, e é importante avaliar pontos como a qualidade das bases, comprimento e quantidade de reads, e presença de adaptadores e contaminantes.
<br><br>
Para este tutorial, acessaremos o <a href="https://www.ncbi.nlm.nih.gov/sra">NCBI SRA</a> para download de dados e utilizaremos os softwares FastQC e Trimmomatic (<a href="https://usegalaxy.eu/">na versão online no Galaxy</a> ou instalados em uma distribuição Linux). Se você ainda não tem estes softwares instalados, pode encontrar instruções <a href="https://cursogenomicaegeneticaufpr.netlify.app/docs/download/">aqui</a>.
<br><br>
Ao utilizar estes softwares e servidores, cite as seguintes referências ou agradecimentos:
<br><br>
<table style="text-align:center; vertical-align:middle;">
  <tr>
    <th><strong>Software</strong></th>
	<th width="300" ><strong>Referência / Agradecimento</strong></th>
  <tr>
    <td>FastQC</td>
    <td>Andrews S, 2019. FastQC: a quality control tool for high throughput sequence data. Disponível online em: <a href="https://www.bioinformatics.babraham.ac.uk/projects/fastqc/">https://www.bioinformatics.babraham.ac.uk/projects/fastqc/</a></td>
  </tr> 
  <tr>
    <td>Trimmomatic</td>
    <td>Bolger AM, Lohse M, Usadel B, 2014. Trimmomatic: A flexible trimmer for Illumina Sequence Data. <b>Bioinformatics</b> 30, 2114-2120. DOI: <a href="https://doi.org/10.1093/bioinformatics/btu170">10.1093/bioinformatics/btu170</a></td>
  <tr>
    <td>Galaxy Europe</td>
    <td><i>“The authors acknowledge the support of the Freiburg Galaxy Team: Person X and Björn Grüning, Bioinformatics, University of Freiburg (Germany) funded by the <a href="http://www.sfb992.uni-freiburg.de/">Collaborative Research Centre 992 Medical Epigenetics</a> (<a href="http://www.dfg.de/">DFG</a> grant SFB 992/1 2012) and the German Federal Ministry of Education and Research <a href="http://www.bmbf.de/">BMBF</a> grant 031 A538A <a href="https://www.denbi.de/">de.NBI</a>-RBC.”</i></td>
  </tr>
</table> 
<br><br>
Há diversas diferenças no processo de avaliação e processamento de dados brutos entre Illumina e PacBio. Uma das principais diferenças é a existência de softwares de montagem de genomas e transcriptoma que usam reads de PacBio e são capazes de utilizar diretamente os reads brutos, pois possuem um sistema de correção de erros próprio. Sendo assim, neste tutorial falaremos apenas da avaliação e processamento de reads Illumina, cujas etapas não estão embutidas nos softwares de montagem. Informações sobre o processamento e correção de erros de reads de PacBio estarão presentes no tutorial de montagem de genomas.
<br><br>
Clique nos links abaixo para acessar as páginas do NCBI SRA que contém os arquivos que serão utilizados nesta e em atividades práticas futuras:
<br><br>
<ul>
<li><a href="https://www.ncbi.nlm.nih.gov/sra/SRX6433203%5baccn%5d">Sequenciamento de genoma de <i>Phyllosticta citriasiana</i> (linhagem CBS 120486) em Illumina</a></li>
<li><a href="https://www.ncbi.nlm.nih.gov/sra/SRX9601541%5baccn%5d">Sequenciamento de genoma de Phyllosticta citricarpa (linhagem CBS 111.20) em PacBio</a></li>
</ul>
Clique nos links abaixo para baixar os arquivos que serão utilizados nesta prática:
<br><br>
<li><a href="https://github.com/desirrepetters/cursogenomicaegenetica.ufpr/raw/master/userguide/content/pt-br/docs/praticas/example_files/aula_02/aula_02.zip">Link para pasta com todos os arquivos (formato zip)</a></li>
<br>
Para baixar os arquivos individualmente:
<br><br>
<ul>
<li><a href="https://github.com/desirrepetters/cursogenomicaegenetica.ufpr/raw/master/userguide/content/pt-br/docs/praticas/example_files/aula_02/SRR9672751_1_FastQC_1a_Avaliacao.html">Arquivo de saída da primeira avaliação do FastQC para os reads forward de <i>P. citriasiana</i> CBS 120486 (formato HTML)</a></li>
<li><a href="https://github.com/desirrepetters/cursogenomicaegenetica.ufpr/raw/master/userguide/content/pt-br/docs/praticas/example_files/aula_02/SRR9672751_2_FastQC_1a_Avaliacao.html">Arquivo de saída da primeira avaliação do FastQC para os reads reverse de <i>P. citriasiana</i> CBS 120486 (formato HTML)</a></li>
<li><a href="https://github.com/usadellab/Trimmomatic/raw/main/adapters/TruSeq3-PE.fa">Arquivo com adaptadores Illumina para uso no Trimmomatic (formato FASTA)</a></li>
<li><a href="https://github.com/desirrepetters/cursogenomicaegenetica.ufpr/raw/master/userguide/content/pt-br/docs/praticas/example_files/aula_02/Trimmomatic_LOG.txt">Arquivo log do Trimmomatic (formato TXT)</a></li>
<li><a href="https://github.com/desirrepetters/cursogenomicaegenetica.ufpr/raw/master/userguide/content/pt-br/docs/praticas/example_files/aula_02/SRR9672751_1_R1_paired_FastQC_2a_Avaliacao.html">Arquivo de saída da segunda avaliação do FastQC para os reads forward pareados de <i>P. citriasiana</i> CBS 120486 (formato HTML)</a></li>
<li><a href="https://github.com/desirrepetters/cursogenomicaegenetica.ufpr/raw/master/userguide/content/pt-br/docs/praticas/example_files/aula_02/SRR9672751_1_R1_unpaired_FastQC_2a_Avaliacao.html">Arquivo de saída da segunda avaliação do FastQC para os reads forward não pareados de <i>P. citriasiana</i> CBS 120486 (formato HTML)</a></li>
<li><a href="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/example_files/aula_02/SRR9672751_2_R2_paired_FastQC_2a_Avaliacao.html">Arquivo de saída da segunda avaliação do FastQC para os reads reverse pareados de <i>P. citriasiana</i> CBS 120486 (formato HTML)</a></li>
<li><a href="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/example_files/aula_02/SRR9672751_2_R2_unpaired_FastQC_2a_Avaliacao.html">Arquivo de saída da segunda avaliação do FastQC para os reads reverse não pareados de <i>P. citriasiana</i> CBS 120486 (formato HTML)</a></li>
</ul>
Nesta atividade prática iremos:
<br><br>
<ul>
<li>Realizar o download de dois conjuntos de dados de sequenciamento de genoma presentes no NCBI SRA e convertê-los ao formato FASTQ</li>
<li>Avaliar a qualidade dos reads, comprimento e quantidade de reads do arquivo Illumina</li>
<li>Detectar a presença de adaptadores do arquivo Illumina</li>
<li>Filtrar reads de baixa qualidade e remover adaptadores do arquivo Illumina</li>
<li>Reavaliar o arquivo filtrado para confirmar a efetividade da filtragem do arquivo Illumina</li>
</ul>
Após a realização destas etapas, utilizaremos os arquivos obtidos para as <a href="https://cursogenomicaegeneticaufpr.netlify.app/docs/praticas/aula_03">atividades práticas de montagem de genomas.</a>
</div>

## Obtenção de sequências no NCBI SRA e conversão para o formato FASTQ no ambiente Linux

<div align="justify">
O <a href="https://www.ncbi.nlm.nih.gov/sra">NCBI Sequence Read Archive</a> é uma base de dados do National Center for Biotechnology Information, contendo dados de sequenciamento de nova geração. Lá é possível encontrar diversos tipos de dados, das mais variadas tecnologias de sequenciamento (como Illumina, PacBio e Oxford Nanopore Technology) e vários tipos de estudos (sequenciamento de genomas, transcriptoma, estudos de metagenômica, entre outros). A existência do SRA é extremamente importante, seja para lidar com o crescente volume de dados de sequenciamento de larga escala que são gerados, como para fornecer acesso e ferramentas de trabalho adequadas aos pesquisadores que trabalham com estes dados, e garantir que as informações estejam amplamente e permanentemente disponíveis.
<br><br>
Apesar da grande variedade de dados disponíveis no NCBI SRA, no contexto deste tutorial curso nosso foco será nas páginas e ferramentas destinadas à obtenção dados brutos de sequenciamento de genomas.
<br><br>
No topo página inicial do NCBI SRA existe uma ferramenta de busca para encontrarmos os dados de interesse. Na caixa de seleção suspensa, a opção “SRA” estará selecionada, para restringir a pesquisa apenas aos dados relacionados à esta base. Ao clicar na caixa de seleção suspensa, são listadas várias opções, e se forem selecionadas, realizarão pesquisas em outras bases do NCBI. Para iniciar a pesquisa, digitamos os termos de interesse no campo em branco e clicamos em “Search” para pesquisar. Podemos procurar pelo nome dos organismos de interesse, por nomes de genes, ou também realizar combinações de palavras como nome do organismo e interesse e nomes de genes:
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_1.png" alt="Caixa de busca na página inicial do NCBI SRA" align="center">
</center>
<br><br>
Na próxima página podemos ver o número de resultados da nossa busca (neste caso, 23), navegar por diferentes páginas de resultados (neste caso, 2 páginas) ou abrir resultados específicos. Neste exemplo, podemos ver que há dados de sequenciamento de transcriptoma (1) e sequenciamento de genoma (2). Abaixo de cada título, existe uma breve descrição da metodologia de sequenciamento utilizada, quantidade de bases e tamanho do arquivo. Vamos abrir o segundo resultado, referente ao sequenciamento de um genoma, para avaliar algumas das informações principais e descobrir como obter os dados de sequenciamento:
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_2.png" alt="Resultados da busca pelo termo phyllosticta citricarpa no NCBI SRA" align="center">
</center>
<br><br>
Ao abrir o primeiro resultado, é possível descobrir qual é o código de acesso com o qual o conjunto de dados foi registrado (neste caso, SRX9601541), e obter mais informações sobre o sequenciamento e o depositante, tais como:
<br><br>
<ol>
<li><b>Submitted by: </b>responsável pela submissão dos dados, nesse caso “<i>DOE Joint Genome Institute (JGI)</i>”</li>
<li><b>Study: </b>título breve do estudo, nesse caso “<i><u>Phyllosticta citricarpa</u> CBS 111.20 Standard Draft genome sequencing</i>”, referindo-se ao sequenciamento de genoma rascunho da linhagem CBS 111.20 da espécie <i>Phyllosticta citricarpa</i></li>
<li><b>Sample: </b>amostra utilizada, nesse caso “<i><u>Phyllosticta citricarpa</u> CBS 111.20</i>”. Nesse caso a informação já estava presente no título, mas em muitos casos o título é mais abrangente e não informa qual linhagem foi sequenciada.</li>
<li><b>Library: </b>aqui é possível encontrar informações sobre a estratégia de sequenciamento, como:<br>
<ol style="list-style-type: lower-alpha; padding-bottom: 0;">
<li><b>Name: </b>nome da biblioteca sequenciada</li>
<li><b>Instrument: </b>tipo de equipamento utilizado, neste caso “<i>PacBio RS II</i>” da Pacific BioSciences</li>
<li><b>Strategy: </b>estratégia ou objetivo do sequenciamento, neste caso “<i>WGS (Whole Genome Sequencing)</i>", ou seja, sequenciamento do genoma completo</li>
<li><b>Selection: </b>informações sobre possível seleção de fragmentos ou regiões antes do sequenciamento. Nesse caso “<i>Random</i>”, indicando que não houve seleção de regiões preferenciais para sequenciamento.</li>
<li><b>Layout: </b>este campo indica se os reads gerados são simples ou pareados. Neste caso, “<i>Single</i>”, indicando que são reads simples, não pareados.</li>
<li><b>Construction protocol: </b>o tipo de protocolo utilizado para a construção da biblioteca para sequenciamento, se houver. Nesse caso, “<i>Regular (DNA)</i>”, indicando o uso de um protocolo padrão a partir de DNA, sem etapas diferenciadas.</li>
<li><b>Runs: </b>quantas corridas foram sequenciadas ("<i>1 run</i>"), com a quantidade de posições lidas pelo sequenciador ("<i>6.1M spots</i>"), total de bases em todos os reads ("<i>53.4G bases</i>) e tamanho do arquivo para download ("<i>12.6 Gb</i>").</li>
</ol>
</li>
</ol>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_3.png" alt="Página dos dados do sequenciamento de genoma da linhagem CBS 111.20 de P. citricarpa no NCBI SRA, com dados sobre os arquivos" align="center">
</center>
<br><br>
Para seguir para o download da sequência via NCBI SRA, podemos clicar no código de acesso da corrida (nesse caso, “<i>SRR13162647</i>”). Na página da corrida, podemos ter acesso à diferentes abas. Na aba “<i>Metadata</i>” os dados com a quantidade de posições lidas pelo sequenciador, total de bases em todos os reads e tamanho do arquivo para download são novamente apresentados.
<br><br>
Para realizar o download, podemos seguir para a aba “<i>Data access</i>”: 
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_4.png" alt="Página dos dados referentes ao código de acesso SRR13162647 do NCBI SRA, evidenciando as abas Metadata e Data access" align="center">
</center>
<br><br>
Na aba “<i>Data access</i>” a primeira opção de download é no formato SRA, que é um arquivo produzido pelo NCBI SRA a partir dos dados brutos que foram enviados. Esse tipo de formato não pode ser processado diretamente pelos softwares de análise e processamento de reads, e precisa ser convertido para FASTQ ou SAM com ferramentas do SRA Toolkit, que veremos em etapas futuras.
<br><br>
Para fazer o download diretamente para o computador, basta clicar no link do arquivo no NCBI ou AWS e escolher o local de preferência para salvar o arquivo. O NCBI também fornece acesso aos dados no formato SRA pela plataforma em nuvem GCP (para usuários que possuam uma conta), e no formato original FASTQ nos serviços AWS e GCP (também exigindo uma conta de usuário.
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_5.png" alt="Aba Data access do registro SRR13162647 dos dados de sequenciamento de genoma da linhagem CBS 111.20 de Phyllosticta citricarpa, evidenciando o link para download dos dados" align="center">
</center>
<br><br>
Embora exista a possibilidade realizar o download do arquivo SRA pelo navegador, na maior parte dos casos os arquivos possuem um tamanho grande e podem ocorrer erros no processo de download. Nesse sentido, uma recomendação para ambientes Linux é fazer o download diretamente por linha de comando. Há algumas possibilidades:
<br><br>
Usando o software “<i>wget</i>” e o link de download:
<br><br>
</div>

```
wget https://sra-download.ncbi.nlm.nih.gov/traces/sra50/SRR/012854/SRR13162647
```

<div align="justify">
Usando o software “<i>curl</i>”:
<br><br>
</div>

```
curl -O https://sra-download.ncbi.nlm.nih.gov/traces/sra50/SRR/012854/SRR13162647
```

<div align="justify">
Usando a ferramenta “<i>prefetch</i>” do SRA Toolkit:
<br><br>
</div>

```
prefetch SRR13162647
```

<div align="justify">
Como o arquivo obtido está no formato SRA, ainda precisamos convertê-lo para o formato FASTQ antes de prosseguir com as avaliações e processamentos. Para isso, utilizaremos a ferramenta “<i>fastd-dump</i>” do SRA Toolkit:
<br><br>
Para corridas com layout “<i>SINGLE</i>”:
<br><br>
</div>

```
fastq-dump SRR13162647
```

<div align="justify">
Para corridas com layout “<i>PAIRED</i>”, como no caso dos arquivos da linhagem CBS 120486 de de Phyllosticta citriasiana, existe a possiblidade de separar os reads de orientação forward e reverse em arquivos distintos:
<br><br>
</div>

```
fastq-dump --split-files SRR9672751
```

<div align="justify">
Existem duas possibilidades para uso do fastq-dump em relação ao arquivo de entrada. É possível pular a etapa anterior de download e informar o código de acesso diretamente ao comando (como nos exemplos acima) e a ferramenta para o download do arquivo diretamente do NCBI para realizar a conversão (e salvará uma cópia do arquivo SRA na pasta $HOME/ncbi/public/sra/). Caso não deseje pular o download e queira utilizar o arquivo baixado, basta informar a localização do arquivo dentro dos diretórios ao escrever o comando. Por exemplo, se o arquivo tivesse sido baixado na pasta “RNAseq” dentro da pasta “home”, o script seria:
<br><br>
</div>

```
fastq-dump --split-files /home/RNAseq/SRR9672751
```

<div align="justify">
Em todos os modos de uso do fastq-dump para corridas com layout PAIRED serão gerados arquivos de saída: nesse exemplo, SRR9672751_1 para os reads forward e SRR9672751_2 para os reads reverse.
<br><br>
</div>

## Obtenção de sequências no NCBI SRA e conversão para o formato FASTQ no Galaxy

<div align="justify">
Uma das vantagens de trabalhar com as análises diretamente no <a href="https://usegalaxy.eu/">Galaxy</a> é a possibilidade de importar os dados do NCBI SRA diretamente às pastas do Galaxy e realizar a conversão para o formato FASTQ sem ter que realizar o download do arquivo no computador. Para iniciar o download, acesse o Galaxy e crie uma nova “<i>History</i>”. Ao criar a nova “<i>History</i>”, clique na opção “<i>get data from an external source</i>”:
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_6.png" alt="Opção 'get data from an external source' na aba History do Galaxy" align="center">
</center>
<br><br>
Na aba “<i>Tools</i>”, na região esquerda da página, surgirão várias possibilidades de download de arquivos em diferentes bases de dados. Selecione a opção “<i>Faster Download and Extract Reads in FASTQ format from NCBI SRA</i>”:
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_7.png" alt="Opção 'Faster Download and Extract Reads in FASTQ format from NCBI SRA' na aba Tools do Galaxy" align="center">
</center>
<br><br>
Na opção “<i>select input type</i>” selecione “<i>SRR accession</i>”, e digite o código de acesso na opção “<i>Acession</i>”. Na aba “<i>Advanced Options</i>”, na opção “<i>Select how to split the spots</i>” podemos escolher se desejamos separar os reads forward e reverse em arquivos distintos, quando os dados são pareados. Como essa informação é importante, iremos escolher a opção “<i>--split-files: write reads into different files (forward and reverse may not match if one read is empty</i>”, separando os reads forward e reverse em arquivos diferentes.  Existem algumas opções de processamento avançadas para filtrar os dados, mas como iremos realizar avaliações e filtragens posteriormente, podemos deixar estas opções em branco e clicar em “<i>Execute</i>” para iniciar o download:
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_8.png" alt="Opções para configuração da ferramenta Faster Download and Extract Reads in FASTQ format from NCBI SRA no Galaxy" align="center">
</center>
<br><br>
Quando o download estiver finalizado, os arquivos estarão listados na aba History com uma cor verde, e poderão ser utilizados e selecionados em análises seguintes com outras ferramentas: 
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_9.png" alt="Download de arquivos finalizado no Galaxy, indicado pela coloração verde nos itens da lista" align="center">
</center>
<br><br>
</div>

## Primeira avaliação de qualidade no FastQC

<div align="justify">
Com os reads em mãos, iremos avaliar sua qualidade, comprimento, quantidade, e presença de possíveis adaptadores usados para o sequenciamento e contaminantes utilizando o software FastQC.
<br><br>
O FastQC é um software que realiza uma avaliação de qualidade de reads de sequenciamento de larga escala a fim de identificar problemas que possam ter ocorrido tanto durante o sequenciamento, quanto no material que foi sequenciado (como contaminações).  Após a avaliação, o FastQC gera um arquivo HTML ilustrando diferentes parâmetros que foram avaliados, e seus respectivos resultados. Após a instalação, iremos rodar o FastQC para os dados da linhagem CBS 120486 de <i>Phyllosticta citriasiana</i> com o seguinte comando:
<br><br>
</div>

```
fastqc SRR9672751_1 SRR9672751_2
```

<div align="justify">
Ao terminar a análise, podemos abrir o arquivo HTML que foi gerado em qualquer navegador e avaliar os resultados. Para cada um dos módulos, o FastQC classifica os resultados em:
<br><br>

<table style="text-align:center; vertical-align:middle;">
  <tr>
  <td><img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/green_arrow.png" alt="Seta verde do FastQC" align="center" width="50"></td>
  <td width="700"><b><i>Normal:</i></b> nenhuma alteração do que seria esperado em um contexto normal</td>
  </tr>
  <tr>
  <td><img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/orange_sign.png" alt="Ponto de exclamação laranja do FastQC" align="center" width="50"></td>
  <td><b><i>Slightly abnormal:</i></b> levemente anormal, com algumas leves alterações em relação ao que seria esperado em um contexto normal, mas que provavelmente são alterações simples e fáceis de se resolver, ou que não devem ser motivo de maiores cuidados</td>
  </tr>
  <tr>
  <td><img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/red_error.png" alt="Erro vermelho do FastQC" align="center" width="50"></td>
  <td><b><i>Very unusual:</i></b> muito anormal, com muita diferença em relação ao que seria esperado em um contexto normal, e que pode caracterizar problemas mais sérios, e demandar mais atenção</td>
  </tr>
  </table>
  <br><br>
É importante frisar que tais avaliações do FastQC devem ser sempre contextualizadas em relação à amostra que está sendo trabalhada, pois a comparação é feita em relação a um contexto normal e aleatório. Por exemplo, o FastQC espera que parâmetros como quantidade e tamanho de reads sigam uma distribuição normal, por exemplo. Entretanto, se você está trabalhando com um conjunto de dados que sofreu seleção em função do tamanho dos reads, provavelmente este parâmetro não apresentará uma distribuição normal e será classificado como “slightly abnormal” ou “very unusual”, mas isso não significará que existam grandes problemas. De forma geral, é importante sempre encarar as avaliações como uma visão geral dos dados e levar o contexto sempre em consideração antes de realizar a interpretação.
<br><br>
Prosseguiremos para a avaliação do arquivo HTML e cada um dos módulos analisados pelo FastQC, usando o arquivo HTML de saída para os reads forward (<b>SRR9672751_1</b>) como exemplo:
<br><br>
</div>

### Basic Statistics (Estatísticas básicas)

<div align="justify"> 
Este primeiro módulo sempre é classificado como “Normal” (ícone verde) e fornece algumas estatísticas básicas sobre o arquivo analisado, e nunca tais como:
<br><br>
<ul>
<li><b><i>Filename:</i></b> nome original do arquivo que foi analisado (nesse caso, <b>SRR9672751_1.gz</b>)</li>
<li><b><i>File type:</i></b> informa se o arquivo continha informações sobre a qualidade das bases ou teve que ser convertido para que essa informação fosse obtida (nesse caso, “<b>Conventional base calls</b>”, informando que os dados estavam no formato convencional, contendo a informação sobre a qualidade das bases)</li>
<li><b><i>Encoding:</i></b> informa se a codificação ASCII de qualidade das bases foi encontrada no arquivo, e qual tipo de codificação foi utilizada, visto que diferentes plataformas podem usar codificações distintas (nesse caso, “<b>Sanger / Illumina 1.9</b>”, informando que a codificação ASCII segue o padrão utilizado usualmente para Sanger ou Illumina 1.9)</li>
<li><b><i>Total sequences:</i></b> número total de sequências (reads) presentes no arquivo (nesse caso, “<b>20.773.714</b>”)</li>
<li><b><i>Sequences flagged as poor quality:</i></b> quantidade de sequências que apresentam qualidade ruim (nesse caso, “<b>0</b>”)</li>
<li><b><i>Sequence Length:</i></b> informa o tamanho da menor e da maior sequência (read) presentes no arquivo, e se todos os reads forem do mesmo tamanho, apenas um valor será mostrado (nesse caso, todos os reads possuem tamanho de “<b>150</b>” bases)</li>
<li><b><i>%GC:</i></b> porcentagem de GC considerando todas as bases e todos os reads (nesse caso, o conteúdo GC médio é de “<b>52%</b>”)</li>
</ul>
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_10.png" alt="Resultados do módulo Basic Statistics do FastQC" align="center">
</center>
<br>
</div>

### Per Base Sequence Quality (Qualidade da sequência em cada uma das bases)

<div align="justify">
Este módulo fornece uma visão geral da variação da qualidade das bases ao longo das posições dos reads, fornecendo um diagrama de caixa (box-whisker plot) para cada uma das posições. Para cada plot é utilizada a seguinte notação:
<br><br>
<ul>
<li><b>Linha vermelha central:</b> mediana da qualidade das bases</li>
<li><b>Caixa amarela:</b> intervalo entre o primeiro e o terceiro quartil (25 - 75% dos valores)</li>
<li><b>Linhas verticais (<i>whisker</i>):</b> traçadas até os pontos representando 10% e 90% da distribuição</li>
<li><b>Linha azul:</b> média da qualidade das bases</li>
<li><b>Eixo x:</b> posição da base ao longo read</li>
<li><b>Eixo y:</b> valor de qualidade, quanto mais alto, melhor. No fundo, as cores verde, laranja e vermelho dividem os valores de qualidade em muito bom, razoável e ruim</li>
</ul>

A classificação dos resultados desse módulo é a seguinte:
<br><br>


Em geral, as classificações “<i>Slightly abnormal</i>” e “<i>Very unusual</i>” para este módulo ocorrem pela degradação de qualidade que normalmente é observada nas corridas devido à degradação dos reagentes, e será observada à medida que a quantidade de reads aumenta ou o comprimento é aumentado. 
<br><br>
Como solução de problemas, é possível cortar e filtrar os reads em função da qualidade (quando as bases de má qualidade estão restritas ao começo ou final da sequência, é possível remover regiões específicas e manter as regiões de boa qualidade). Entretanto, quando todas as bases apresentam qualidade ruim, possivelmente será necessário sequenciar a amostra novamente.
<br><br>
No exemplo em questão (análise do arquivo SRR9672751_1), podemos perceber que para a maioria das posições ao longo dos reads os dados apresentam boa qualidade, com a mediana e toda a distribuição na faixa verde, e que de uma forma geral a qualidade dos reads é ótima. Entretanto, a qualidade vai diminuindo perto do final alguns dos reads (o que é comum que aconteça). 
<br><br>
Como a mediana e o quartil inferior para esses reads finais ainda estão acima de 25, percebemos que essa perda de qualidade não deve ter acontecido em muitos reads, e o FastQC não classificou esta observação como problemática. <a href="https://cursogenomicaegeneticaufpr.netlify.app/docs/praticas/aula_02/#filtragem-e-limpeza-dos-reads-no-trimmomatic">Na atividade prática com o Trimmomatic iremos explicar em maiores detalhes como filtrar estas poucas posições de menor qualidade</a>.
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_5.png" alt="Campo superior esquerdo dos resultados de blastn" align="center">
</center>
<br><br>
Para nós, as informações mais relevantes estão presentes na seção seguinte: “<i>Sequences producing significant alignments</i>”, em que são listadas as sequências mais similares à sequência consenso que foram encontradas pelo BLASTn. Ao lado da lista de sequências, há algumas colunas com informações adicionais. Vamos discorrer brevemente sobre seu significado.
<br><br>
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_6.png" alt="Campo ~Sequences producing significant alignments~ na página de resultados de blastn, com diversas sequências de Fusarium" align="center">
</center>
<br><br>
Dentre as colunas descritivas, podemos ver duas colunas que se referem ao “<i>score</i>”. Este score depende do tamanho do alinhamento, do número de matches, mismatches, gaps e da matriz de comparação de sequências utilizada e é normalizado através de variáveis estatísticas. 
<br><br>
Como vimos na aula teórica, ao alinhar duas sequências há vários alinhamentos possíveis, dependendo da combinação de gaps, matches e mismatches existentes. Além disso, diferentes matrizes de substituição ou penalidades de gaps podem gerar diferentes scores. Resumidamente, o score se refere à qualidade geral daquele alinhamento entre as duas sequências, mas não deve ser interpretado no sentido de que a sequência analisada é mais similar às sequências de maior score.
<br><br>
“<i>Query coverage</i>” se refere à porcentagem da sequência que foi utilizada na busca está presente em cada um dos alinhamentos. Em muitos casos, a sequência que estamos analisando pode ser mais longa ou mais curta que as sequências dos bancos de dados, de modo que parte dela pode ficar de fora do alinhamento e reduzir a porcentagem de cobertura. 
<br><br>
Já o E-value nos dá uma noção estatística sobre a qualidade do alinhamento entre as sequências: ele representa a quantidade de alinhamentos como scores iguais ou maiores ao score apresentado espera-se que ocorram ao acaso em uma base de dados de mesmo tamanho que a base de dados utilizada. Se o E-value é baixo, significa que é altamente improvável que um alinhamento de score igual aconteça ao acaso, sugerindo que de fato as sequências são similares. Já se o E-value é muito alto, significa que provavelmente as sequências não são similares, já que um alinhamento de mesmo score é altamente provável, e, portanto, pode ter acontecido por mera coincidência (e não tem real significado biológico). 
<br><br>
A coluna “<i>Percentage of identity (Per. Ident)</i>” se refere à porcentagem de similaridade entre as duas sequências alinhadas, exclusivamente para as regiões alinhadas (representadas na porcentagem de “<i>Query coverage</i>”. Isso significa que é possível que sequências diferentes apresentem a mesma porcentagem de cobertura, mas porcentagens de identidade diferente (uma é mais semelhante à sequência analisada do que a outra), ou uma sequência apresentam menor porcentagem de cobertura, mas porcentagem de identidade maior (provavelmente um pedaço da sequência ficou de fora do alinhamento pela diferença entre o comprimento da sequência analisada e a sequência no banco de dados, mas a porção que foi incluída no alinhamento tem maior similaridade do que a outra que apresenta maior porcentagem de cobertura).
<br><br>
E, por fim, a coluna Accession se refere ao código de acesso com que cada uma das sequências foi registrada ao ser depositada na base de dados, e que pode ser utilizado para buscar diretamente cada uma das sequências, bem como obter informações mais detalhadas sobre elas (tais como dados do indivíduo sequenciado, autores que realizaram o depósito da sequência, entre outras).
<br><br>
De forma geral, os valores das colunas descritivas não devem ser interpretados como valores que dão suporte ou não à identificação de um indivíduo num nível taxonômico tão preciso como espécie. O mais adequado é utilizá-los para avaliar a qualidade daqueles alinhamentos e avaliar se sequenciamos de fato o gene correto, e qual a identificação taxonômica de uma forma mais abrangente (por exemplo, em termos de família ou gênero).
<br><br>
No nosso exemplo, é possível ver que em todas as sequências aparece “<i>translation elongation fator 1-alpha (tef1) gene</i>” ao lado da espécie e do isolado, indicando que a região sequenciada na sequência consenso corresponde a um fragmento do fator de elongamento da tradução 1-alpha, bastante utilizado em alguns grupos taxonômicos. Como a intenção ao sequenciar este indivíduo era de fato sequenciar este fragmento, sabemos então que fomos bem sucedidos em termos de sequenciar a região correta.
<br><br>
Por outro lado, vemos diversas espécies listadas: “<i>Fusarium awaxy</i>”, “<i>Fusarium cf. fujikuroi</i>”, “<i>Fusarium</i> sp.”, “<i>Fusarium culmorum</i>”, “<i>Fusarium subglutinans</i>”, “<i>Fusarium guttiforme</i>”, “<i>Fusarium antophilum</i>”. Este tipo de ocorrência é bastante comum e não é um exemplo isolado: aqui demonstramos o quão problemático é realizar a identificação de espécies apenas via BLASTn, sem análises mais refinadas. Neste caso, a única informação relevante para nossa atividade é de que provavelmente o indivíduo da sequência consenso pertence ao gênero “<i>Fusarium</i>”, mas uma identificação definitiva só poderá ser realizada por meio de análise filogenética utilizando sequências de referência do gênero Fusarium. Não há informações que nos permitam escolher uma das espécies da lista e definir a identificação do indivíduo da sequência consenso. 
<br><br>
Agora que definimos que é necessário obter sequências das espécies descritas e aceitas de <i>Fusarium</i> para realizar a identificação do indivíduo da sequência consenso, seguiremos para a etapa de busca e obtenção de sequências no NCBI GenBank.
<br><br>
</div>

## Obtendo sequências no NCBI GenBank

<div align="justify">
O NCBI GenBank é a base de dados do National Center for Biotechnology Information, contendo dados genéticos disponíveis publicamente. Lá é possível encontrar diversos tipos de dados, desde sequências de DNA de diferentes regiões genômicas, montagens de genomas completos, transcriptomas, entre outros. A existência de bases de dados como o GenBank é extremamente importante, seja para lidar com o crescente volume de dados biológicos gerados, como para fornecer acesso e ferramentas de trabalho adequadas aos pesquisadores que trabalham com estes dados, e garantir que as informações estejam amplamente e permanentemente disponíveis.
<br><br>
Apesar da grande variedade de dados disponíveis no GenBank, no contexto do curso nosso foco será nas páginas e ferramentas destinadas à obtenção de sequências de DNA.
<br><br>
No topo página inicial do GenBank existe uma ferramenta de busca para encontrarmos os dados de interesse. Ao clicar na caixa de seleção suspensa, são listadas várias opções. Para sequências de DNA, escolheremos “<i>Nucleotide</i>”. Em seguida, digitamos os termos de interesse no campo em branco e clicamos em Search para pesquisar. Podemos procurar pelo nome dos organismos de interesse, por nomes de genes, ou também realizar combinações de palavras como nome do organismo e interesse e nomes de genes: 
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_7.png" alt="Campo superior de buscas no NCBI GenBank com a opção ~Nucleotide~ selecionada e ~fusarium tef1~ como termo de busca" align="center">
</center>
<br><br>
Na próxima página podemos ver o número de resultados da nossa busca (neste caso, 7070), navegar por diferentes páginas de resultados (neste caso, 354 páginas) ou abrir resultados específicos. Vamos abrir o primeiro resultado para avaliar algumas das informações principais e descobrir como obter sua sequência FASTA:
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_8.png" alt="Resultado da busca pelo termo ~fusarium tef1~no NCBI GenBank, com 7070 resultados no total e 354 páginas de resultados." align="center">
</center>
<br><br>
Ao abrir o primeiro resultado, é possível descobrir qual é o código de acesso com o qual a sequência foi registrada (neste caso, <b>MG183712.1</b>), avaliar seu tamanho (714 bp), data em que foi depositada (15 de abril de 2018), qual a identificação segundo o depositante, quem são os autores e se a sequência faz parte de algum trabalho publicado ou não. Neste caso, a sequência consta como “Não publicada”.
<br><br>
Para baixar a sequência há duas possibilidades. A primeira consiste em clicar em “<i>Send to</i>” na parte superior direita, selecionar “<i>Complete record</i>”, “<i>File</i>”, formato “<i>FASTA</i>” e “<i>Create file</i>” para fazer download da sequência no formato FASTA. 
<br><br>
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_9.png" alt="Página da sequência de código MG183712.1 de Fusarium solani" align="center">
</center>
<br><br>
A segunda possibilidade é clicar em FASTA logo abaixo do código GenBank da sequência e abrir uma nova janela em que a sequência no formato FASTA poderá ser copiada:
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_10.png" alt="Página da sequência de código MG183712.1 de Fusarium solani, em formato FASTA" align="center">
</center>
<br><br>
Para organizar as sequências, seja baixando os arquivos ou copiando cada uma, nossa sugestão é criar um documento no Notepad ++, e editar os cabeçalhos para remover informações extras, caracteres especiais e espaços. Uma boa abordagem é manter os cabeçalhos das sequências sempre como “>[Gênero]_[epíteto específico]_[código do isolado]”, para facilitar a compatibilidade entre diferentes softwares. No caso do exemplo, editaríamos a sequência de <b>“>MG183712.1 Fusarium solani strain FJAT-31354 TEF1 (TEF1) gene, partial cds”</b> para <b>“>Fusarium_solani_FJAT_31354”</b>. É importante sempre usar underlines ao invés de espaços. 
<br><br>
Como você já deve ter percebido pela quantidade de resultados que aparecem na busca, pelo status de “<i>Unpublished</i>” de muitas sequências, e pelo fato da identificação da sequência ser dependente do depositante (e sem curadoria posterior), muitas vezes buscar diretamente pelos genes ou nome dos organismos não é a estratégia mais fácil para filtrar e selecionar as sequências adequadas a serem incluídas na análise filogenética. 
<br><br>
Em geral, uma abordagem mais acertada é buscar por artigos ou outros bancos de dados que informem quais são as espécies aceitas, quais são as linhagens-tipo e os respectivos códigos GenBank para suas sequências, e aí utilizando estes códigos, fazer a busca e baixar as sequências conforme demonstramos anteriormente. 
<br><br>
No caso da sequência consenso, pertencente ao gênero <i>Fusarium</i>, a informação está dispersa em diversos materiais na literatura, e no contexto do curso não seria produtivo que todos gastássemos tempo reunindo essas informações. Além disso, como o gênero <i>Fusarium</i> apresenta muitas espécies descritas, muitas vezes se torna mais fácil realizar análises separando o gênero em diversos complexos de espécies. Como as espécies listadas nos primeiros alinhamentos no nosso resultado de BLASTn pertencem ao mesmo complexo, focaremos a análise no complexo “<i>Fusarium fujikuroi</i>”. Vamos trabalhar com um conjunto de sequências de referência do complexo <i>Fusarium fujikuroi</i> que já foi reunido e curado manualmente, cujos códigos e informações sobre os isolados (origem geográfica, substrato de isolamento, coleção em que foram depositados e referências dos estudos que descreveram cada espécie) estão <a href="https://github.com/desirrepetters/cursogenomicaegenetica.ufpr/raw/master/userguide/content/pt-br/docs/praticas/example_files/aula_02/Aula_02_Tabela.pdf">nesta tabela</a>. 
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_11.png" alt="Cabeçalho da Tabela das Sequências de Referência" align="center">
</center>
<br><br>
Este tipo de tabela é extremamente útil não só para a nossa própria organização em relação ao nosso material de estudo, mas também para auxiliar outros pesquisadores a terem um acesso mais fácil a estas informações. Nesse sentido, aconselhamos que no momento de pesquisa na literatura, procurem por este tipo de tabela (que facilitará imensamente seu trabalho futuro). Caso este tipo de tabela não exista e as informações estejam muito dispersas em vários bancos de dados e artigos, talvez você inicialmente passe por um processo trabalhoso de reunião e curadoria de informação. Nesse caso, que tal contribuir com outros pesquisadores e facilitar o acesso deles à esta informação ao incluir a tabela em seu trabalho? Com certeza você auxiliará diversas pessoas e esse esforço será reconhecido com diversas citações à sua tabela!
<br><br>
</div>

## Escolhendo o outgroup e organizando o arquivo de entrada

<div align="justify">
Vamos então produzir o alinhamento múltiplo unindo a sequência consenso ainda não identificada, as sequências de referência, e um outgroup adequado. Como já discutimos durante a aula teórica, o outgroup adequado consiste em um indivíduo externo ao grupo que está sendo analisado, mas que compartilhe um ancestral comum. Neste caso, há algumas possibilidades:
<br><br>
<ul>
<li>Algum representante de um gênero que pertença à família Nectriaceae (mesma família do gênero <i>Fusarium</i>);</li>
<li>Algum indivíduo do gênero <i>Fusarium</i>, mas que pertença a um complexo de espécies distinto do complexo <i>Fusarium fujikuroi</i></li>
</ul>
<br><br>
Na nossa atividade prática seguiremos a segunda opção, utilizando um indivíduo da espécie <i>Fusarium oxysporum</i>, que pertence ao complexo de espécies <i>Fusarium oxysporum</i>.
<br><br>
Para produzir o alinhamento múltiplo, utilizaremos o software MAFFT. Podemos utilizar tanto a <a href="https://mafft.cbrc.jp/alignment/server/">versão online</a> quanto o plugin do PhyloSuite, e demonstraremos como realizar o alinhamento das duas formas. Primeiro, devemos criar um único arquivo FASTA contendo todas as sequências que queremos alinhar. Podemos fazer isso abrindo todos os arquivos no Notepad ++, copiar e colar todas as sequências num novo documento e salvar com a extensão FASTA.
<br><br>
</div>

## Alinhamento múltiplo com o MAFFT online

<div align="justify">
Usando o <a href="https://mafft.cbrc.jp/alignment/server/">MAFFT online</a>, no primeiro campo podemos colar diretamente as sequências de DNA a serem alinhadas ou selecionar o arquivo FASTA que acabamos de criar:
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_12.png" alt="Campo para inserir ou escolher o arquivo de sequências no MAFFT online" align="center">
</center>
<br><br>
Em seguida, podemos configurar algumas opções referente ao arquivo de saída. Para evitar problemas de sequências em orientações opostas no meio do alinhamento, podemos selecionar a opção “<i>Adjust direction according to the first sequence</i>”, em que a orientação das sequências será ajustada para seguir a mesma orientação da primeira sequência do alinhamento. Também é possível pedir que o MAFFT ordene as sequências no arquivo de saída de acordo com a sua similaridade com a opção “<i>Aligned</i>” dentro de “<i>Output order</i>”. Ao selecionar essa opção, as sequências mais similares estarão mais próximas no arquivo final, o que é bastante conveniente para a inspeção visual do alinhamento.
<br><br>
Também é possível dar um nome à tarefa em “<i>Job name</i>” e inserir seu e-mail para ser notificado quando o alinhamento ficar pronto, visto que o procedimento pode ser demorado para arquivos muito grandes com muitas sequências longas.
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_13.png" alt="Opções para o alinhamento com o MAFFT online" align="center">
</center>
<br><br>
O MAFFT também fornece algumas opções avançadas para otimização dos alinhamentos em caso de muitas sequências ou regiões problemáticas, e também a possibilidade de utilizar diferentes matrizes de substituição e alterar a penalidade de gaps. Para este exemplo, utilizaremos as opções já definidas por padrão e clicaremos em “<i>Submit</i>”.
<br><br>
Ao finalizar o alinhamento, o MAFFT redireciona para uma página de resultados. É possível avaliar se havia sequências em orientação reversa no arquivo original na opção “<i>Open all plots</i>”. Se houverem somente linhas vermelhas nos gráficos, todas as sequências estavam em orientação direta (quando comparadas à primeira sequência do alinhamento). Se houver alguma linha azul, esta sequência estava em orientação reversa quando comparada à primeira sequência do alinhamento. Para baixar o alinhamento em formato FASTA, basta clicar em “<i>Fasta format</i>” no menu superior. Neste caso, salvaremos o alinhamento como “<b>Aula_02_Alinhamento_MAFFT.fasta</b>”.
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_14.png" alt="Instruções para download do alinhamento produzido pelo MAFFT online" align="center">
</center>
<br><br>
</div>

## Alinhamento múltiplo com o plugin do MAFFT no PhyloSuite

<div align="justify">
Também é possível produzir o alinhamento pelo plugin do MAFFT no PhyloSuite. Caso ainda não tenha o PhyloSuite instalado e o plugin do MAFFT configurado, siga estas instruções <a href="https://cursogenomicaegeneticaufpr.netlify.app/docs/phylosuite">aqui</a>. Após a instalação e configuração, para fazer os alinhamentos primeiramente iremos definir uma pasta de trabalho ao abrir o programa. Clicando no ícone amarelo, escolha a pasta que contiver o arquivo com as sequências. Lembre-se de <b>NÃO</b> selecionar a opção “<i>Use this as the default and do not ask again</i>” para que você sempre seja capaz de escolher qualquer pasta de seu interesse e não ficar restrito à uma única pasta:
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_15.png" alt="Janela de seleção do workplace no PhyloSuite" align="center">
</center>
<br><br>
Em seguida, clique na opção “<i>MAFFT</i>” dentro do menu “<i>Alignment</i>":
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_16.png" alt="Opção MAFFT dentro do menu Alignment do PhyloSuite" align="center">
</center>
<br><br>
Na janela de opções, escolha o arquivo com todas as sequências, modo de alinhamento “<i>Normal</i>”, estratégia “<i>1. –auto</i>” e Export Format “<i>3. FASTA Format / Sorted</i>” para exportar o arquivo final para o formato FASTA com as sequências ordenadas por similaridade. Em “<i>Additional parameters</i>” geralmente a opção “--adjustdirection” para ajustar a orientação das sequências para a mesma da primeira sequência já está selecionada, mas se não estiver, deve ser selecionada. As outras opções não precisam ser selecionadas. Em seguida, aperte em “<i>Start</i>” e acompanhe o processo por meio da barra de progresso:
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_17.png" alt="Janela de configurações do MAFFT dentro do PhyloSuite" align="center">
</center>
<br><br>
Em alguns casos o programa pode retornar uma janela de erro, mas apesar disso, o alinhamento foi concluído. Para encontrar o arquivo de saída, basta acessar a pasta “<i>GenBank file</i>”, em seguida “<i>files</i>”, “<i>mafft_results</i>” e a pasta com a data e horário em você utilizou o software. Dentro desta pasta estará o arquivo FASTA alinhado pelo MAFFT e um arquivo log da tarefa.
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_18.png" alt="Possível bug inofensivo ao utilizar o MAFFT no PhyloSuite" align="center">
</center>
<br><br>
</div>

## Inspeção visual e edição do alinhamento no MEGA

<div align="justify">
Para inspeção visual e edição, abriremos o alinhamento no software MEGA 7 ou MEGA X. Para abrir o alinhamento seleciona a opção “<i>Edit/Build Aligment</i>” dentro do menu “<i>Align</i>”. Escolha a opção “<i>Retrieve sequences from a file</i>", clique em OK e escolha o arquivo do alinhamento no seu computador (seja o produzido pelo MAFFT online ou pelo plugin do MAFFT no PhyloSuite) para abrir:
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_19.png" alt="Instruções para abrir um arquivo de alinhamento no MEGA" align="center">
</center>
<br><br>
Ao abrir o alinhamento na aba do Explorador de Alinhamentos, vemos que nem todas as sequências tem o mesmo comprimento: algumas são mais longas que outras, de modo que as mais curtas acabam possuindo vários “gaps” no começo e no final. Nesse sentido, é importante cortar um pouco do início e do final do alinhamento para que o alinhamento consista de regiões representadas pela maioria das sequências. Podemos selecionar e deletar estas bases do começo e do final da mesma forma que fizemos ao produzir a sequência consenso na <a href="https://cursogenomicaegeneticaufpr.netlify.app/docs/praticas/aula_02/#o-que-%C3%A9-e-como-analisar-um-eletroferograma">Aula 01</a>.
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_20.png" alt="Corte nas porções inicial e final do alinhamento" align="center">
</center>
<br><br>
Para a inspeção visual e edição ao longo de um alinhamento, observe principalmente se:
<br><br>
<b>Há um excesso de transversões?</b> Em geral, transições são mais prováveis e frequentes, de modo que isso deve se refletir no alinhamento. Se ao longo de um alinhamento você encontrar um excesso de transversões, avalie estas regiões com mais cuidado para ver se não seria possível incluir alguns gaps ou deslocar algumas bases para que se alinhem à outras.
<br><br>
<b>Há bases que estão deslocadas, e uma região próxima à qual claramente elas deveriam estar alinhadas?</b> Nesse caso, desloque as bases e corrija o alinhamento. No exemplo a seguir, o gap existente nas sequências 15 a 20 provavelmente não existe, visto que nas sequências logo abaixo ou logo acima a região deslocada existe. Nesse caso, selecione as bases deslocadas e mova para a posição correta com a opção “<i>Move selected block left</i>” do menu superior:
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_21.png" alt="Opção ~Move selected block left~ no MEGA 7" align="center">
</center>
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_22.png" alt="Exemplo de bases deslocadas em um alinhamento" align="center">
</center>
<br><br>
No contexto deste tutorial não iremos discutir exaustivamente todos os pontos do alinhamento que poderiam ou não ser editados. A edição dos alinhamentos exigirá um pouco de prática e tempo, e à medida que você trabalhar com frequência com as mesmas sequências, e olhar com atenção o alinhamento, este processo se tornará cada vez mais fácil e você terá mais confiança nas edições. Lembre-se: sempre é possível desfazer as mudanças (Ctrl + Z), ou exportar várias opções de edição, testá-las em sua análise filogenética e perceber as mudanças, ou mesmo não fazer nenhuma edição enquanto não sentir confiança o suficiente para isso.
<br><br>
Após inspecionar e estar satisfeito com sua edição, exporte o alinhamento no formato FASTA. Dentro do menu superior “<i>Data</i>”, selecione a opção “<i>FASTA format</i>” dentro de “<i>Export alignment</i>”. Escolha a pasta do computador e o nome de sua preferência para salvar o arquivo. Na próxima atividade, faremos o teste de modelo evolutivo e produziremos nossas primeiras árvores filogenéticas.
<br><br>
</div>

## Filtragem e limpeza dos reads no Trimmomatic

## Aula em vídeo

<br>
<div align="center">
<iframe width="560" height="315" src="https://www.youtube.com/embed/yz0njYN0oIw" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> 
<br><br>
Clique <a href="https://photos.app.goo.gl/pukRgyGobNrkH3Un7">aqui</a> para fazer o download do vídeo.
<br><br>
</div>