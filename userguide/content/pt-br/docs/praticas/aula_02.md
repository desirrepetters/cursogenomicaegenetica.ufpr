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
	<th width="600" ><strong>Referência / Agradecimento</strong></th>
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
Em todos os modos de uso do fastq-dump para corridas com layout PAIRED serão gerados arquivos de saída: nesse exemplo, <b>SRR9672751_1</b> para os reads forward e <b>SRR9672751_2</b> para os reads reverse.
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

<table style="vertical-align:middle;">
  <tr>
  <td style="text-align:center" width="100"><img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/green_arrow.png" alt="Seta verde do FastQC" align="center" width="50"></td>
  <td width="650"><b><i>Normal:</i></b> nenhuma alteração do que seria esperado em um contexto normal</td>
  </tr>
  <tr>
  <td style="text-align:center"><img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/orange_sign.png" alt="Ponto de exclamação laranja do FastQC" align="center" width="50"></td>
  <td><b><i>Slightly abnormal:</i></b> levemente anormal, com algumas leves alterações em relação ao que seria esperado em um contexto normal, mas que provavelmente são alterações simples e fáceis de se resolver, ou que não devem ser motivo de maiores cuidados</td>
  </tr>
  <tr>
  <td style="text-align:center"><img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/red_error.png" alt="Erro vermelho do FastQC" align="center" width="50"></td>
  <td><b><i>Very unusual:</i></b> muito anormal, com muita diferença em relação ao que seria esperado em um contexto normal, e que pode caracterizar problemas mais sérios, e demandar mais atenção</td>
  </tr>
  </table>
  <br><br>
É importante frisar que tais avaliações do FastQC devem ser sempre contextualizadas em relação à amostra que está sendo trabalhada, pois a comparação é feita em relação a um contexto normal e aleatório. Por exemplo, o FastQC espera que parâmetros como quantidade e tamanho de reads sigam uma distribuição normal, por exemplo. Entretanto, se você está trabalhando com um conjunto de dados que sofreu seleção em função do tamanho dos reads, provavelmente este parâmetro não apresentará uma distribuição normal e será classificado como “slightly abnormal” ou “very unusual”, mas isso não significará que existam grandes problemas. De forma geral, é importante sempre encarar as avaliações como uma visão geral dos dados e levar o contexto sempre em consideração antes de realizar a interpretação.
<br><br>
Prosseguiremos para a avaliação do arquivo HTML e cada um dos módulos analisados pelo FastQC, usando o arquivo HTML de saída para os reads forward (<b>SRR9672751_1</b>) como exemplo.
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
<br>
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
<table style="vertical-align:middle;">
  <tr>
  <td style="text-align:center" width="100"><img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/green_arrow.png" alt="Seta verde do FastQC" align="center" width="50"></td>
  <td width="650"><b><i>Normal:</i></b> nenhuma alteração do que seria esperado em um contexto normal</td>
  </tr>
  <tr>
  <td style="text-align:center"><img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/orange_sign.png" alt="Ponto de exclamação laranja do FastQC" align="center" width="50"></td>
  <td><b><i>Slightly abnormal:</i></b> o quartil inferior (25%) para qualquer base corresponde a um valor de qualidade menor que 10, ou a mediana do valor de qualidade de qualquer base é menor que 25</td>
  </tr>
  <tr>
  <td style="text-align:center"><img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/red_error.png" alt="Erro vermelho do FastQC" align="center" width="50"></td>
  <td><b><i>Very unusual:</i></b> o quartil inferior (25%) para qualquer base corresponde a um valor de qualidade menor que 5, ou a mediana do valor de qualidade de qualquer base é menor que 20</td>
  </tr>
  </table>
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
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_11.png" alt="Resultados do módulo Per base sequence quality do FastQC" align="center">
</center>
<br><br>
De modo geral, as bases dos reads do sequenciamento da linhagem CBS 120486 de <i>Phyllosticta citriasiana</i> são de excelente qualidade e poderão ser bem aproveitadas em sua maioria para análises posteriores. Mas como ficaria o gráfico caso essa não fosse a situação? 
<br><br>
Apresentamos então um exemplo de dados em que a qualidade é muito ruim. Nesse outro exemplo, várias posições apresentam os quartis inferiores das distribuições dos valores de qualidades na faixa laranja (razoável) e vermelha (ruim), inclusive com valores abaixo de 5, sugerindo que a qualidade dos reads é bastante questionável.
<br><br>
Nesse tipo de cenário, diferente do exemplo em <i>P. citriasiana</i>, dificilmente será possível aproveitar esses reads após filtragem e processamento, pois muitos dos reads serão completamente removidos, e os que permanecerem provavelmente serão cortados em quase toda a sua extensão, restando pouca informação biológica. Nesse caso, a melhor solução será sequenciar o material novamente, com mais cuidado nos procedimentos de extração, purificação e sequenciamento, evitando erros que possam causar o mesmo tipo de problema.
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_12.png" alt="Exemplo de resultados ruins no módulo Per base sequence quality do FastQC" align="center">
</center>
<br>
</div>

### Per Sequence Quality Scores (Qualidade por sequência / read) 

<div align="justify">
Este módulo permite avaliar a qualidade média dos reads, e detectar se existe ou não alguns reads que possuem qualidade inferior ao longo de todo o comprimento da sequência. Neste plot, o eixo x representa o valor médio de qualidade (quanto maior, melhor), e o eixo y representa o número de reads. A linha vermelha traça a distribuição dos valores de qualidade ao longo de todos os reads, e o formato da curva permite detectar se existem porções de reads que apresentam qualidade inferior. 
<br><br>
Em um cenário ideal, apenas um pico nos melhores valores de qualidade será observado. Entretanto, não é incomum observar que alguns reads tenham qualidade inferior (por limitações do próprio equipamento), desde que representam uma porção muito pequena do total de reads sequenciados.
<br><br>
A classificação dos resultados desse módulo é a seguinte:
<br><br>
<table style="vertical-align:middle;">
  <tr>
  <td style="text-align:center" width="100"><img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/green_arrow.png" alt="Seta verde do FastQC" align="center" width="50"></td>
  <td width="650"><b><i>Normal:</i></b> o valor de qualidade médio mais observado é igual ou superior à 27</td>
  </tr>
  <tr>
  <td style="text-align:center"><img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/orange_sign.png" alt="Ponto de exclamação laranja do FastQC" align="center" width="50"></td>
  <td><b><i>Slightly abnormal:</i></b> o valor de qualidade médio mais observado é inferior à 27, sendo correspondente à uma taxa de erro de 0.2%</td>
  </tr>
  <tr>
  <td style="text-align:center"><img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/red_error.png" alt="Erro vermelho do FastQC" align="center" width="50"></td>
  <td><b><i>Very unusual:</i></b> o valor de qualidade médio mais observado é inferior à 20, sendo correspondente à uma taxa de erro de 1%</td>
  </tr>
  </table>
  <br><br>
Em geral, as classificações “<i>Slightly abnormal</i>” e “<i>Very unusual</i>” para este módulo ocorrem pela degradação de qualidade que normalmente é observada nas corridas, e por limitações do próprio equipamento. 
<br><br>
Como solução de problemas, é possível cortar e filtrar os reads em função da qualidade, removendo reads com qualidade baixa, quando a quantidade de reads de baixa qualidade não for muito elevada. Entretanto, quando muitos reads apresentarem qualidade ruim, a solução está relacionada ao equipamento e não ao processamento de dados.
<br><br>
No exemplo em questão (análise do arquivo SRR9672751_1), podemos perceber que a maior parte dos reads apresentam excelente qualidade, com o pico da distribuição com qualidade entre 35 e 36, e pouquíssimos reads de qualidade inferior a 25. Se os reads forem filtrados em função de sua qualidade média, a maior parte dos reads não será removida.
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_13.png" alt="Resultados do módulo Per sequence quality scores do FastQC" align="center">
</center>
<br><br>
De modo geral, assim como discutimos no módulo anterior, as bases dos reads do sequenciamento da linhagem CBS 120486 de <i>Phyllosticta citriasiana</i> são de excelente qualidade e poderão ser bem aproveitadas em sua maioria para análises posteriores.  
<br><br>
Mas como ficaria o gráfico caso essa não fosse a situação? Apresentamos então um exemplo de dados em que a qualidade é muito ruim. Nesse outro exemplo, podemos perceber que há vários reads de qualidade baixa, começando numa qualidade média de 9, com muitos reads de valor de qualidade inferior a 25, e o valor máximo de 33, inferior ao exemplo de <i>P. citriasiana</i> (valor máximo de 37). 
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_14.png" alt="Exemplo de resultados ruins do módulo Per sequence quality scores do FastQC" align="center">
</center>
<br><br>
Nesse tipo de situação, diferente do exemplo em <i>P. citriasiana</i>, dificilmente será possível aproveitar esses reads após filtragem e processamento, pois se filtramos os reads em função de sua qualidade média, muitos dos reads serão completamente removidos, restando poucos reads para análise. Nesse caso, a melhor solução será sequenciar o material novamente, com mais cuidado nos procedimentos de extração, purificação e sequenciamento, evitando erros que possam causar o mesmo tipo de problema.
<br>
</div>

### Per base sequence content (Conteúdo de sequência em cada uma das bases)

<div align="justify">
Este módulo fornece uma visão geral do conteúdo das bases (A, T, C ou G) ao longo das posições dos reads, plotando a posição do read no eixo x, e a porcentagem das bases no eixo y. Em um conjunto aleatório, a diferença na proporção entre as bases seria mínima, de modo que a linha de cada uma das bases seria paralela às outras. 
<br><br>
A classificação dos resultados desse módulo é a seguinte:
<br><br>
<table style="vertical-align:middle;">
  <tr>
  <td style="text-align:center" width="100"><img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/green_arrow.png" alt="Seta verde do FastQC" align="center" width="50"></td>
  <td width="650"><b><i>Normal:</i></b> diferenças entre as proporções de A, T, C ou G não foram observadas ou, se foram, foram menores que 10% </td>
  </tr>
  <tr>
  <td style="text-align:center"><img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/orange_sign.png" alt="Ponto de exclamação laranja do FastQC" align="center" width="50"></td>
  <td><b><i>Slightly abnormal:</i></b> a diferença entre a proporção entre A, T, C ou G é maior que 10% para pelo menos uma posição dos reads</td>
  </tr>
  <tr>
  <td style="text-align:center"><img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/red_error.png" alt="Erro vermelho do FastQC" align="center" width="50"></td>
  <td><b><i>Very unusual:</i></b> a diferença entre a proporção entre A, T, C ou G é maior que 20% para pelo menos uma posição dos reads</td>
  </tr>
  </table>
  <br><br>
Em geral, as classificações “<i>Slightly abnormal</i>” e “<i>Very unusual</i>” para este módulo ocorrem por desvios do que seria esperado em condições aleatórias, mas que não costumam causar problemas em análises posteriores. Novamente, aqui se reforça a importância de ter sempre o contexto dos dados e das análises em mente para detectar se a classificação deste módulo realmente representa um problema. As causas de desvios podem ser:
<br><br>
<ul>
<li><b>Sequências super representadas:</b> seja por razões biológicas ou por procedimentos de sequenciamento ou amostragem, se algumas sequências específicas forem mais frequentes que as outras podem causar desvios nas distribuições. </li>
<li><b>Viés na fragmentação ou na preparação da biblioteca para sequenciamento:</b> algumas estratégias ou kits de sequenciamento possuem procedimentos de preparação que podem gerar vieses na composição das bases no começo ou final das sequências, mas que não atrapalham as análises e processamento posterior dos dados</li>
<li><b>Remoção de adaptadores:</b> o corte e filtragem das sequências para remoção de adaptadores também pode inserir um viés na composição das bases no começo ou final da sequência, mas também não atrapalha as análises e processamento posterior dos dados. </li>
</ul>

Todas estas questões dificilmente serão resolvidas com processamento e filtragem de reads (principalmente porque o viés no conteúdo de bases pode ser originado no próprio sequenciamento), mas em geral não atrapalham análises posteriores, e podem ser deixadas de lado dependendo do contexto.
<br><br>
No exemplo em questão (análise do arquivo <b>SRR9672751_1</b>), podemos perceber que o FastQC classificou como “<i>slightly abnormal</i>”, pois no início dos reads (até a posição 5) há alguns pequenos desvios. Entretanto, como já discutimos anteriormente, esta observação pode estar relacionada ao próprio processo de sequenciamento e inclusive representar sequências de adaptadores. Nesse caso, não caracteriza um problema com a amostra, e podemos utilizar os dados sem problemas.
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_15.png" alt="Resultados do módulo Per base sequence content do FastQC" align="center">
</center>
<br><br>
Mas como ficaria o gráfico caso essa não fosse a situação? Apresentamos então um exemplo de dados em há grandes desvios entre as proporções das bases, e em que os desvios ocorrem ao longo de toda a sequência:
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_16.png" alt="Exemplo de resultados ruins do módulo Per base sequence content do FastQC" align="center">
</center>
<br><br>
Nesse outro exemplo, é importante ter em mente o tipo de contexto e situação para definir uma solução. Se os dados forem provenientes de um experimento ou condição em que se esperam vieses no tipo de sequências, fragmentação ou tamanho das sequências (por exemplo, um experimento de super expressão de algum gene em relação a outros), esse tipo de observação pode ser normal e é possível prosseguir com a análise sem problemas. 
<br><br>
Por outro lado, se não for esse o caso, pode indicar problemas no sequenciamento (como por exemplo, a corrida sequenciou apenas os adaptadores ou sequenciou somente um tipo ou grupo de sequências). Nesse caso, a melhor solução será sequenciar o material novamente, com mais cuidado nos procedimentos de extração, purificação e sequenciamento, evitando erros que possam causar o mesmo tipo de problema.
</div>

### Per sequence GC content (Conteúdo GC por sequência)

<div align="justify">
Este módulo mede a distribuição do conteúdo GC ao longo dos reads e compara com uma distribuição normal teórica. No eixo x é plotada a porcentagem de GC, e no eixo y é plotada a quantidade de reads, enquanto a distribuição teórica é plotada na linha azul, e a distribuição observada plotada na linha vermelha.  Em um conjunto normal e aleatório, uma distribuição bastante similar à distribuição teórica seria observada.
<br><br>
A classificação dos resultados desse módulo é a seguinte:
<br><br>
<table style="vertical-align:middle;">
  <tr>
  <td style="text-align:center" width="100"><img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/green_arrow.png" alt="Seta verde do FastQC" align="center" width="50"></td>
  <td width="650"><b><i>Normal:</i></b> desvios em relação à distribuição normal não foram observados, e se foram, a soma dos desvios representam menos de 15% dos reads</td>
  </tr>
  <tr>
  <td style="text-align:center"><img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/orange_sign.png" alt="Ponto de exclamação laranja do FastQC" align="center" width="50"></td>
  <td><b><i>Slightly abnormal:</i></b> a soma dos desvios em relação à distribuição normal representa mais de 15% dos reads</td>
  </tr>
  <tr>
  <td style="text-align:center"><img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/red_error.png" alt="Erro vermelho do FastQC" align="center" width="50"></td>
  <td><b><i>Very unusual:</i></b> a soma dos desvios em relação à distribuição normal representa mais de 30% dos reads</td>
  </tr>
  </table>
  <br><br>
Em geral, as classificações “<i>Slightly abnormal</i>” e “<i>Very unusual</i>” para este módulo ocorrem por desvios do que seria esperado em condições aleatórias, como problemas no sequenciamento ou preparo da biblioteca, contaminação (dois ou mais picos ao longo do gráfico) ou um viés biológico da própria amostra que não caracterizaria um problema em análises posteriores. Novamente, aqui se reforça a importância de ter sempre o contexto dos dados e das análises em mente para detectar se a classificação deste módulo realmente representa um problema a ser resolvido.  
<br><br>
A depender do contexto e tipo de problema, dificilmente é solucionado com o processamento dos dados, e pode exigir um novo sequenciamento do material (como no caso de problemas de sequenciamento, preparo ou contaminação).
<br><br>
No exemplo em questão (análise do arquivo <b>SRR9672751_1</b>), podemos perceber que o FastQC classificou como “<i>slightly abnormal</i>”, pois há um leve desvio em relação à distribuição teórica esperada. Entretanto, como já discutimos anteriormente, esta observação pode estar relacionada ao próprio processo de sequenciamento ou por questões biológicas da própria amostra. Nesse caso, apesar do pequeno desvio, percebemos que o pico ainda está relacionado ao mesmo valor do pico de GC% da distribuição teórica. Além disso, o pico é único, não existindo picos adicionais que seriam evidência de possíveis contaminações. Sendo assim, essa observação provavelmente não caracteriza um problema com a amostra, e podemos utilizar os dados sem problemas.
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_17.png" alt="Resultados do módulo Per sequence GC content do FastQC" align="center">
</center>
<br><br>
Mas como ficaria o gráfico caso essa não fosse a situação? Apresentamos então um exemplo de dados em há um desvio mais significativo em relação à distribuição teórica esperada, contendo mais de um pico ao longo da distribuição.  
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_18.png" alt="Exemplo de resultados ruins do módulo Per sequence GC content do FastQC" align="center">
</center>
<br><br>
Nesse outro caso também é importante ter em mente o tipo de contexto e situação para definir uma solução. Se os dados forem provenientes de um experimento ou condição em que se esperam vieses no tipo de sequências e sequências com diferentes tipos de conteúdo GC são mais frequentes que outras (por exemplo, um experimento de super expressão de algum gene em relação a outros), esse tipo de observação pode ser normal e é possível prosseguir com a análise sem problemas. 
<br><br>
Por outro lado, se não for esse o caso, pode indicar problemas no sequenciamento e inclusive a presença de contaminantes. Nesse caso, a melhor solução será sequenciar o material novamente, com mais cuidado nos procedimentos de extração, purificação e sequenciamento, evitando erros que possam causar o mesmo tipo de problema, com especial atenção à possíveis fontes de contaminação (como amostras ou reagentes contaminados).
</div>

### Per base N content (Quantidade de N em cada uma das bases)

<div align="justify">
Quando o sequenciador não consegue determinar com confiabilidade qual é a base presente numa posição, geralmente adicionará um N à sequência. Sendo assim, esse módulo informa a porcentagem de bases indeterminadas (N) para cada posição ao longo dos reads. No eixo x, o gráfico apresenta a posição ao longo do read, e no eixo y, a porcentagem de reads com N para a respectiva posição, com a linha vermelha representando a distribuição observada.
<br><br>
A classificação dos resultados desse módulo é a seguinte:
<br><br>
<table style="vertical-align:middle;">
  <tr>
  <td style="text-align:center" width="100"><img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/green_arrow.png" alt="Seta verde do FastQC" align="center" width="50"></td>
  <td width="650"><b><i>Normal:</i></b> nenhum N foi observado, ou se foi, nenhuma das posições apresenta uma quantidade de N maior que 5%</td>
  </tr>
  <tr>
  <td style="text-align:center"><img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/orange_sign.png" alt="Ponto de exclamação laranja do FastQC" align="center" width="50"></td>
  <td><b><i>Slightly abnormal:</i></b> pelo menos uma das posições possui uma quantidade de N maior que 5%.</td>
  </tr>
  <tr>
  <td style="text-align:center"><img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/red_error.png" alt="Erro vermelho do FastQC" align="center" width="50"></td>
  <td><b><i>Very unusual:</i></b> pelo menos uma das posições possui uma quantidade de N maior que 20%.</td>
  </tr>
  </table>
  <br><br>
  Em geral, as classificações “<i>Slightly abnormal</i>” e “<i>Very unusual</i>” para este módulo ocorrem pela degradação de qualidade que normalmente é observada nas corridas devido à degradação dos reagentes, e será observada à medida que a quantidade de reads aumenta ou o comprimento é aumentado, então é importante olhar os resultados deste módulo junto com os resultados dos outros, a fim de observar se este resultado tem relação com o módulo <a href="https://cursogenomicaegeneticaufpr.netlify.app/docs/praticas/aula_02/#per-base-sequence-quality-qualidade-da-sequência-em-cada-uma-das-bases">“Per base sequence quality”</a>.
  <br><br>
  Como solução de problemas, é possível cortar e filtrar os reads em função da qualidade, pois os N serão classificados como bases de qualidade ruim (quando as bases de má qualidade estão restritas ao começo ou final da sequência, é possível remover regiões específicas e manter as regiões de boa qualidade). Entretanto, quando todas as bases apresentam N, possivelmente será necessário sequenciar a amostra novamente.
  <br><br>
  No exemplo em questão (análise do arquivo SRR9672751_1), podemos perceber que nenhuma base indeterminada ocorreu em nenhuma das posições ao longo de todos os reads, reforçando <a href="https://cursogenomicaegeneticaufpr.netlify.app/docs/praticas/aula_02/#per-base-sequence-quality-qualidade-da-sequência-em-cada-uma-das-bases">a observação anterior de que a qualidade das bases é excelente e não há bases duvidosas</a>.
  <br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_19.png" alt="Resultados do módulo Per base N content do FastQC" align="center">
</center>
<br><br>
Mas como ficaria o gráfico caso essa não fosse a situação? Apresentamos então um exemplo de dados em existem bases indeterminadas em algumas posições de alguns reads (cerca de 5% dos reads apresentam bases indeterminadas entre as posições 26 e 30):
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_20.png" alt="Exemplo de resultados ruins do módulo Per base N content do FastQC" align="center">
</center>
<br><br>
Neste exemplo ainda é possível cortar e filtrar os reads em função da qualidade, pois somente uma quantidade pequena de reads apresentam bases indeterminadas, e estão restritas a uma porção da sequência total. Entretanto, quando todas as bases apresentam N, ou quando uma grande quantidade de reads apresenta esse tipo de problema, será necessário sequenciar a amostra novamente.
</div>

### Sequence length distribution (Distribuição de tamanho das sequências)

<div align="justify">
Em geral, os procedimentos de preparação da amostra, fragmentação e sequenciamento levam à geração de reads de tamanhos uniformes. No entanto, variações podem surgir, e a remoção de bases de má qualidade também pode encurtar alguns reads. Este módulo apresenta um gráfico de distribuição do tamanho observado dos reads, plotando o comprimento da sequência em pares de bases no eixo x, e quantidade de reads no eixo y. Na maioria dos casos apenas um pico será observado (no tamanho de fragmento associado à estratégia de sequenciamento).
<br><br>
A classificação dos resultados desse módulo é a seguinte:
<br><br>
<table style="vertical-align:middle;">
  <tr>
  <td style="text-align:center" width="100"><img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/green_arrow.png" alt="Seta verde do FastQC" align="center" width="50"></td>
  <td width="650"><b><i>Normal:</i></b> a distribuição de tamanho das sequências é uniforme</td>
  </tr>
  <tr>
  <td style="text-align:center"><img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/orange_sign.png" alt="Ponto de exclamação laranja do FastQC" align="center" width="50"></td>
  <td><b><i>Slightly abnormal:</i></b> todas as sequências tem tamanhos distintos</td>
  </tr>
  <tr>
  <td style="text-align:center"><img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/red_error.png" alt="Erro vermelho do FastQC" align="center" width="50"></td>
  <td><b><i>Very unusual:</i></b> pelo menos uma da sequência tem um comprimento de 0</td>
  </tr>
  </table>
  <br><br>
Em diversos casos é perfeitamente normal possuir reads de tamanhos distintos dentro do conjunto de dados, seja por questões da própria amostra, e também porque algumas plataformas de sequenciamento de fragmentos mais longos como a PacBio sempre produzem reads de tamanhos variáveis. Sendo assim, as classificações de erro deste módulo podem ser ignoradas, e sua finalidade acaba sendo mais demonstrativa, para fornecer uma visão geral da distribuição de tamanho.
<br><br>
No exemplo em questão (análise do arquivo SRR9672751_1), podemos perceber que a distribuição de tamanho de bases é bastante uniforme, e que todos os reads tem tamanho de 150 bases (determinado pela estratégia de sequenciamento utilizada):
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_21.png" alt="Resultado do módulo Sequence Length Distribution do FastQC" align="center">
</center>
<br><br>
Em um arquivo com reads de tamanhos variáveis, poderíamos observar resultados como esse:
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_22.png" alt="Exemplo de resultados ruins do módulo Sequence Length Distribution do FastQC" align="center">
</center>
</div>

### Sequence duplication levels (Nível de duplicação das sequências)

<div align="justify">
Num conjunto de dados diverso e aleatório, espera-se que a maior parte das sequências ocorra na mesma proporção que as outras, sem que estejam duplicadas. Sendo assim, esse módulo avalia o nível de duplicação de cada um dos reads no conjunto de dados e cria um plot para mostrar a distribuição de possíveis sequências duplicadas. No eixo x o gráfico apresenta os níveis de duplicação, e no eixo y a porcentagem de sequências de cada categoria. A linha azul representa a distribuição observada para a amostra original, e a linha vermelha representa a distribuição que seria observada se as sequências observadas fossem removidas. Em seu título, o gráfico apresenta a porcentagem de sequências que seriam mantidas caso as sequências duplicadas fossem removidas, para fornecer uma ideia do nível de perda no caso de remoção de sequências.
<br><br>
A classificação dos resultados desse módulo é a seguinte:
<br><br>
<table style="vertical-align:middle;">
  <tr>
  <td style="text-align:center" width="100"><img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/green_arrow.png" alt="Seta verde do FastQC" align="center" width="50"></td>
  <td width="650"><b><i>Normal:</i></b> sequências duplicadas não foram encontradas, ou se foram, correspondem a menos de 20% do conjunto total de sequências</td>
  </tr>
  <tr>
  <td style="text-align:center"><img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/orange_sign.png" alt="Ponto de exclamação laranja do FastQC" align="center" width="50"></td>
  <td><b><i>Slightly abnormal:</i></b> sequências duplicadas compõem mais de 20% do conjunto total de sequências</td>
  </tr>
  <tr>
  <td style="text-align:center"><img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/red_error.png" alt="Erro vermelho do FastQC" align="center" width="50"></td>
  <td><b><i>Very unusual:</i></b> sequências duplicadas compões mais de 50% do conjunto total de sequências</td>
  </tr>
  </table>
  <br><br>
Em diversos cenários é perfeitamente normal que algumas sequências estejam duplicadas. Por exemplo:
<br><br>
<ul>
<li>Em bibliotecas de RNA-Seq, diferentes transcritos apresentam diferentes níveis de expressão, e possivelmente os transcritos mais frequentes serão sequenciados muito mais vezes que transcritos não tão frequentes</li>
<li>Se o sequenciamento está sendo realizado com uma cobertura bastante alta, à medida que todas as regiões do genoma forem sequenciadas, é provável que algumas regiões sejam sequenciadas vária vezes. Nesse tipo de cenário, encontrar muitas sequências duplicadas pode sugerir que a cobertura do sequenciamento não precisava ser tão alta</li>
</ul>

Novamente, aqui se reforça a importância de ter sempre o contexto dos dados e das análises em mente para detectar se a classificação deste módulo realmente representa um problema a ser resolvido, ou se é um comportamento já esperado no tipo de amostra que está sendo analisada.
<br><br>
No exemplo do arquivo SRR9672751_1, a quantidade de sequências duplicadas é muito pequena, e, se forem removidas, 82.6% dos reads ainda serão mantidos na amostra, sugerindo que não há problemas com o arquivo nesse sentido. 
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_23.png" alt="Resultado do módulo Sequence Duplication Levels do FastQC" align="center">
</center>
<br><br> 
Mas como ficaria o gráfico caso essa não fosse a situação? Apresentamos então um exemplo de dados em que o nível de sequências duplicadas é mais alto e que, se fossem removidas, apenas 46.66% permaneceriam no conjunto de dados:
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_24.png" alt="Exemplo de resultados ruins do módulo Sequence Duplication Levels do FastQC" align="center">
</center>
<br><br> 
Nesse outro caso também é importante ter em mente o tipo de contexto e situação para definir uma solução. Se os dados forem provenientes de um experimento ou condição em que se esperam vieses e que algumas sequências sejam mais frequentes que outras (por exemplo, um experimento de super expressão de algum gene em relação a outros), esse tipo de observação pode ser normal e é possível prosseguir com a análise sem problemas. 
<br><br>
Por outro lado, se não for esse o caso, pode indicar problemas no sequenciamento ou com a amostra. Nesse caso, a melhor solução será sequenciar o material novamente, com mais cuidado nos procedimentos de extração, purificação e sequenciamento, evitando erros que possam causar o mesmo tipo de problema.
</div>

### Overrepresented sequences (Sequências super representadas)

<div align="justify">
Num conjunto de dados diverso e aleatório, espera-se que nenhuma sequência única ocorra em uma proporção maior que as outras. Este módulo lista sequências que representem mais de 0.1% do total de sequências, e realiza comparações contra uma lista de contaminantes comuns (e. g.: sequências bacterianas, sequências de adaptadores de sequenciamento) para tentar identificar a identidade das sequências super representadas.
<br><br>
A classificação dos resultados desse módulo é a seguinte:
<br><br>
<table style="vertical-align:middle;">
  <tr>
  <td style="text-align:center" width="100"><img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/green_arrow.png" alt="Seta verde do FastQC" align="center" width="50"></td>
  <td width="650"><b><i>Normal:</i></b> nenhuma sequência única representa mais de 0.1% do total de sequências</td>
  </tr>
  <tr>
  <td style="text-align:center"><img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/orange_sign.png" alt="Ponto de exclamação laranja do FastQC" align="center" width="50"></td>
  <td><b><i>Slightly abnormal:</i></b> pelo menos uma sequência representa mais de 0.1% do total de sequências</td>
  </tr>
  <tr>
  <td style="text-align:center"><img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/red_error.png" alt="Erro vermelho do FastQC" align="center" width="50"></td>
  <td><b><i>Very unusual:</i></b> pelo menos uma sequência representa mais de 1% do total de sequências</td>
  </tr>
  </table>
  <br><br>
Uma sequência pode estar super representada por diversos motivos, sejam biológicos (por exemplo, em experimentos de expressão gênica, podem representar um gene que é muito mais expresso que outros), podem indicar fontes de contaminação, ou que a amostra não era tão diversa quanto se esperava (por exemplo, em estudos de metagenômica, um organismo ou grupo de organismos é muito mais frequente que outros), ou podem representar simplesmente os adaptadores que foram utilizados para o sequenciamento. Levando o contexto do experimento, amostra, e identidade das sequências super representadas, é possível decidir se realmente há um problema com a amostra, ou se já se esperava que algumas sequências fossem mais frequentes que outras.
<br><br>
No exemplo do arquivo <b>SRR9672751_1</b>, o FastQC detectou que há algumas sequências super representadas, mas que correspondem aos adaptadores da plataforma Illumina (<i>“TruSeq Adapter, Index 4 (100% over 50bp”</i>). Nesse caso, será algo simples de resolver através da limpeza e filtragem dos dados, e será possível remover fragmentos que correspondam à adaptadores. Além disso, como estas sequências representam uma pequena fração do conjunto total (0,31% e 0,13%), ainda restarão vários reads que realmente contém informações biológicas:  
<br><br> 
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_25.png" alt="Resultado do módulo Overrepresented sequences do FastQC" align="center">
</center>
<br><br>
Em alguns casos, o FastQC pode encontrar algumas sequências super representadas e não conseguir atribuir uma possível identidade (“<i>No Hit</i>”). Isso pode significar que se tratam de contaminantes biológicos (bactérias em amostras de outros organismos), ou de sequências truncadas, parciais ou de baixa qualidade de adaptadores, que não foram reconhecidas. 
<br><br> 
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_26.png" alt="Exemplo de resultados ruins do módulo Overrepresented sequences do FastQC" align="center">
</center>
<br><br>
Nesse tipo de situação, é importante <a href="https://cursogenomicaegeneticaufpr.netlify.app/docs/praticas/aula_02/#filtragem-e-limpeza-dos-reads-no-trimmomatic">realizar o processamento e filtragem do arquivo e a remoção de adaptadores</a>, e <a href="https://cursogenomicaegeneticaufpr.netlify.app/docs/praticas/aula_02/#segunda-avalia%C3%A7%C3%A3o-de-qualidade-no-fastqc">avaliar se as sequências listadas deixaram de existir</a>. Se ainda persistirem, é interessante investigar a possível identidade das sequências por meio do BLAST, ou seguir para análises posteriores e investigar a contaminação em outros momentos (como na avaliação de montagem de genomas). Por outro lado, dependendo do contexto e do tipo de experimento, estas sequências podem representar sequências reais que realmente estavam mais presentes na amostra que outras. Aqui também reforçamos a importância de lembrar do contexto do experimento ao avaliar os resultados deste módulo.
<br><br>
Um outro tipo de situação problemática ocorre quando todas as sequências super representadas representam sequências de adaptadores, e em alta quantidade, como neste último exemplo. Neste caso, o FastQC conseguiu atribuir a identidade da maior parte dessas sequências, caracterizando-as como adaptadores Illumina. Além disso, elas representam uma fração significativa dos dados, em frequências de 8,12%, 5,08%, 1,08%. Esse tipo de observação sugere que a há contaminação com dímeros de adaptadores, e pode ser necessário sequenciar o material novamente.
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_27.png" alt="Contaminação de dímeros de adaptadores detectada no módulo Overrepresented sequences do FastQC" align="center">
</center>
</div>

### Adapter content (Presença de adaptadores)

<div align="justify">
Muitas das estratégias de sequenciamento de nova geração exigem o uso de adaptadores durante o processo, e parte dos adaptadores acaba sendo sequenciada junto com a amostra. Como estas são sequências técnicas, e não fazem parte da amostra, é necessário detectar se existem, para que sejam removidas. 
<br><br>
Esse módulo avalia a presença de adaptadores para os principais tipos de plataformas de sequenciamento de reads curtos, como Illumina e SOLID. No eixo x, apresenta a posição da base ao longo do read, e no eixo y apresenta a porcentagem de sequências de adaptadores que foi encontrada para cada posição. As linhas coloridas de distribuição representam diferentes plataformas:
<br><br>
<ul>
<li><b><p style="color:#eb3434";>Linha vermelha:</b> Illumina Universal Adapter</li>
<li><b><p style="color:#3437eb";>Linha azul:</b> Illumina Small RNA 3’ Adapter</li>
<li><b><p style="color:#34eb40";>Linha verde:</b> Illumina Small RNA 5’ Adapter</li>
<li><b><p style="color:#000000";>Linha preta:</b> Nextera Transposase Sequence</li>
<li><b><p style="color:#eb34df";>Linha rosa:</b> SOLID Small RNA Adapter</li>
</ul>
A classificação dos resultados desse módulo é a seguinte:
<br><br>
<table style="vertical-align:middle;">
  <tr>
  <td style="text-align:center" width="100"><img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/green_arrow.png" alt="Seta verde do FastQC" align="center" width="50"></td>
  <td width="650"><b><i>Normal:</i></b> nenhum adaptador foi encontrado</td>
  </tr>
  <tr>
  <td style="text-align:center"><img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/orange_sign.png" alt="Ponto de exclamação laranja do FastQC" align="center" width="50"></td>
  <td><b><i>Slightly abnormal:</i></b> em pelo menos uma das posições, adaptadores foram encontrados numa frequência maior que 5%</td>
  </tr>
  <tr>
  <td style="text-align:center"><img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/red_error.png" alt="Erro vermelho do FastQC" align="center" width="50"></td>
  <td><b><i>Very unusual:</i></b> em pelo menos uma das posições, adaptadores foram encontrados numa frequência maior que 10%</td>
  </tr>
  </table>
  <br><br>
Como solução de problemas, é possível remover as bases correspondentes aos adaptadores que estão presentes nos reads, visto que vários softwares de processamento de dados de sequenciamento são capazes de buscar e filtrar os adaptadores. Entretanto, se boa parte das sequências for representada por adaptadores, ou os reads ao longo de todo o seu comprimento forem apenas adaptadores, possivelmente será necessário repetir o sequenciamento do material.
<br><br>
No exemplo do arquivo <b>SRR9672751_1</b>, o FastQC detectou que há sequências de adaptadores nas posições finais de cerca de 10% dos reads, <a href="https://cursogenomicaegeneticaufpr.netlify.app/docs/praticas/aula_02/#overrepresented-sequences-sequ%C3%AAncias-super-representadas">corroborando os resultados do módulo anterior</a>, que listavam alguns adaptadores na lista de sequências super representadas. 
<br><br>
Nesse caso, também será algo simples de resolver através da <a href ="https://cursogenomicaegeneticaufpr.netlify.app/docs/praticas/aula_02/#filtragem-e-limpeza-dos-reads-no-trimmomatic">limpeza e filtragem dos dados</a>, e será possível remover fragmentos que correspondam à adaptadores. Além disso, como poucos reads (10%) apresentam adaptadores, e o adaptadores estão restritos a posições finais dos reads, ainda restarão muitos reads com a informação biológica que nos interessa.
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_28.png" alt="Presença de adaptadores detectada pelo módulo Adapter Content do FastQC" align="center">
</center>
<br><br>
O cenário perfeito neste módulo será não detectar nenhum tipo de adaptador nessa etapa. Ao trabalhar com reads brutos, isso raramente acontece, pois sequenciar parte dos adaptadores é uma característica do processo de sequenciamento. Quando não detectamos adaptadores nesta etapa, isso sugere que os dados já passaram por pelo menos um pré-processamento básico, que removeu os adaptadores.
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_29.png" alt="Ausência de adaptadores informada pelo módulo Adapter Content do FastQC" align="center">
</center>
<br><br> 
Agora, ao finalizar a avaliação de todos os módulos de resultados, podemos perceber que modo geral, os reads do sequenciamento da linhagem CBS 120486 de <i>Phyllosticta citriasiana</i> apresentam ótima qualidade e poderão ser utilizados para outras análises e processos (como a montagem do genoma), desde que realizemos uma filtragem e limpeza para remover os adaptadores que ainda estão presentes.
</div>

## Primeira avaliação de qualidade no FastQC no Galaxy

<div align="justify">
Após realizar o download os dados em formato FASTQ no Galaxy, também é possível realizar a avaliação de qualidade com o FastQC. 
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_30.png" alt="Ferramenta FastQC na aba Tools do Galaxy" align="center">
</center>
<br><br>
Para isso, na aba “<i>Tools</i>”, na região esquerda da página, digite “<i>fastqc</i>” e selecione a opção “<i>FastQC Read Quality Reports</i>”: 
<br><br>
Ao abrir a aba para seleção de arquivos e opções, clique no ícone da opção “<i>Browse Datasets</i>” para escolher os arquivos a serem analisados:
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_31.png" alt="Opção Browse Datasets na ferramenta FastQC no Galaxy" align="center">
</center>
<br><br>
Primeiro, iremos analisar os reads forward, então clique no arquivo correspondente, chamado “<b>SRR9672751_1</b>”:
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_32.png" alt="Janela Browse Datasets, com destaque para o arquivo SRR9672751_1" align="center">
</center>
<br><br>
É possível fornecer uma lista personalizada de contaminantes (“<i>Contaminant list</i>”) ou de adaptadores (“<i>Adapter list</i>”) para o FastQC usar durante a análise, mas nesse momento utilizaremos a lista padrão. Sendo assim, basta clicar em “<i>Execute</i>” para iniciar a tarefa:
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_33.png" alt="Opções para configuração do FastQC no Galaxy" align="center">
</center>
<br><br>
Para analisar o arquivo com reads reverse, clique novamente na opção “<i>Browse Datasets</i>” e clique agora no arquivo “<b>SRR9672751_2</b>”:
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_34.png" alt="Janela Browse Datasets, com destaque para o arquivo SRR9672751_2" align="center">
</center>
<br><br>
Quando ambas as análises estiverem concluídas, os quatro arquivos estarão listados em verde. Para realizar o download, basta clicar sobre o nome do arquivo, e no ícone de disquete, referente à opção “<i>Download</i>”. Os arquivos HTML com os resultados estão listados sob os nomes “<b>FastQC on data: Webpage</b>”.
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_35.png" alt="Análise do FastQC concluída no Galaxy, indicada pela cor verde na lista de arquivos" align="center">
</center>
<br><br>
A <a href="https://cursogenomicaegeneticaufpr.netlify.app/docs/praticas/aula_02/#primeira-avalia%C3%A7%C3%A3o-de-qualidade-no-fastqc">interpretação dos módulos de resultados do arquivo HTML é a mesma descrita anteriormente</a>, na seção em que o uso do FastQC via linha de comando foi descrita.
</div>


## Filtragem e limpeza dos reads no Trimmomatic

<div align="justify">
Após avaliarmos os arquivos HTML referentes aos dados de sequenciamento do genoma da linhagem CBS 120486 de <i>Phyllosticta citriasiana</i>, foi possível perceber que os adaptadores ainda estavam presentes (devido aos avisos nos módulos “<a href="https://cursogenomicaegeneticaufpr.netlify.app/docs/praticas/aula_02/#overrepresented-sequences-sequ%C3%AAncias-super-representadas"><i>Overrepresented sequences</i></a>” e “<a href="https://cursogenomicaegeneticaufpr.netlify.app/docs/praticas/aula_02/#adapter-content-presen%C3%A7a-de-adaptadores"><i>Adapter content</i></a>”). Também observamos avisos nos módulos “<a href="https://cursogenomicaegeneticaufpr.netlify.app/docs/praticas/aula_02/#per-sequence-gc-content-conte%C3%BAdo-gc-por-sequ%C3%AAncia"><i>Per base GC content</i></a>” e “<a href="https://cursogenomicaegeneticaufpr.netlify.app/docs/praticas/aula_02/#per-base-sequence-content-conte%C3%BAdo-de-sequ%C3%AAncia-em-cada-uma-das-bases"><i>Per base sequence content</i></a>”, mas que nesse contexto não representam problemas, e sim provavelmente características da própria amostra, e observamos que embora o módulo “<i>Per base sequence quality</i>” não tenha detectado problemas, a qualidade das bases começava a diminuir perto do final dos reads.
<br><br>
Para garantir que só trabalharemos com as bases de melhor qualidade, e remover os adaptadores que atrapalhariam o processo de montagem de genoma e análise dos dados, iremos processar e filtrar os reads com o software Trimmomatic.
<br><br>
O Trimmomatic é um software que realiza uma avaliação de qualidade de reads de sequenciamento de larga escala a fim de identificar problemas que possam ter ocorrido tanto durante o sequenciamento, quanto no material que foi sequenciado (como contaminações).  Após a avaliação, o FastQC gera um arquivo HTML ilustrando diferentes parâmetros que foram avaliados, e seus respectivos resultados. Após a instalação, é possível rodar o Trimmomatic com o seguinte comando:
<br><br>
</div>

```
java -jar trimmoatic-0.38.jar PE SRR9672751_1.fq.gz SRR9672751_2.fq.gz SRR9672751_1_paired.fq.gz SRR9672751_1_unpaired.fq.gz SRR9672751_2_paired.fq.gz SRR9672751_2_unpaired.fq.gz ILLUMINACLIP:TruSeq3-PE.fa:2:30:10 LEADING:25 SLIDINGWINDOW:4:25 AVGQUAL:25 MINLEN:50
```

<div align="justify">
O Trimmomatic realiza o processamento e filtragem dos reads em uma série de passos diferentes, e sempre na ordem em que os passos foram informados na linha de comando. Recomenda-se sempre primeiro remover os adaptadores antes de realizar as outras etapas de processamento. No comando que utilizamos, as operações realizadas foram:
<br><br>
</div>

### ILLUMINACLIP

<div align="justify">
O comando <i>ILLUMINACLIP</i> remove adaptadores especificados em um arquivo FASTA (nesse caso, arquivo “<a href="https://github.com/usadellab/Trimmomatic/raw/main/adapters/TruSeq3-PE.fa"><b>TruSeq3-PE.fa</b></a>”), e que devem ser compatíveis com a estratégia de sequenciamento utilizada para que possam ser reconhecidos. 
<br><br>
O processo de reconhecimento de adaptadores envolve um trade-off entre sensibilidade (encontrar e remover todas as sequências de adaptadores) e especificidade (não remover nenhuma sequência que não corresponda a um adaptador). Em reads contendo a sequência completa do adaptador este processo é mais simples e pode ser realizado por alinhamentos simples, entretanto, o processo é dificultado quando a sequência do adaptador está parcialmente presente. Para lidar com este problema, o Trimmomatic implementa duas estratégias distintas para identificar adaptadores.
<br><br>
Para iniciar a comparação, o Trimmomatic fragmenta as sequências dos adaptadores em sequências de 16 pares de bases e alinha essas sequências aos reads. Estes alinhamentos curtos são denominados “seeds”. Sendo assim, o primeiro número no argumento <i>ILLUMINACLIP</i> corresponde à quantidade máxima de mismatches que pode existir no alinhamento seed, e se a quantidade de mismatches observada for igual ou menor que esse valor, o alinhamento com a sequência completa do adaptador será realizado. O alinhamento seed é uma forma de otimizar o processo e realizar os alinhamentos completos apenas em casos em que existe uma boa chance de que a sequência realmente seja um adaptador.  e ainda assim tratar a combinação como um match. Nesse exemplo nós usamos “2”, indicando que mesmo quando existirem dois mismatches no alinhamento seed, ela ainda deve ser considerada um potencial adaptador e o alinhamento completo deve ser realizado. 
<br><br>
Em seguida, ao realizar o alinhamento completo, vários fatores são considerados no cálculo do score final. Primeiramente, cada match aumenta o score total em 0.6, e cada mismatch reduz o score em Q/10, sendo Q o valor phred de qualidade da base em questão. Levar a qualidade da base em consideração é importante para reduzir o impacto de mismatches causados por erros de sequenciamento (e que podem não ser reais). Dessa forma, uma sequência de 12 bases que tenha um alinhamento perfeito apresentará um score próximo à 7, enquanto 25 bases são necessárias para um score de 15. Sendo assim, o Trimmomatic recomenda valores entre 7 e 15 como valores mínimos de score de alinhamento para reconhecer um adaptador. Esse valor mínimo é informado no terceiro argumento do comando <i>ILLUMINACLIP</i>, e nesse exemplo usamos “<b>10</b>”, indicando que são necessários pelo menos 16 matches em diferentes bases para identificar um adaptador.
<br><br>
Esta primeira abordagem de identificação é robusta, mas ainda apresenta algumas limitações, uma vez que, embora sequências de adaptadores possam estar localizadas em qualquer posição dos reads, elas geralmente estão presentes em reads provenientes de fragmentos de DNA que eram mais curtos que o tamanho médio de reads da estratégia de sequenciamento utilizada. Por exemplo, se em uma estratégia que produz reads de 150 bases for sequenciado um fragmento de DNA que continha 140 bases, o início do read corresponderá à informação biológica real, mas as 10 últimas bases no final do fragmento corresponderão ao adaptador (“<i>adapter read-through</i>”), e dificilmente seriam identificadas pelo modo de alinhamento simples descrito anteriormente.
<br><br>
Sendo assim, quando porções muito curtas de adaptadores estão presentes, como identificá-las sem perder a especificidade? Para este tipo de situações existe a abordagem palindrômica do Trimmomatic, que se baseia em um fenômeno bastante comum no processo de sequenciamento de bibliotecas pareadas.
<br><br>
Nesse tipo de bibliotecas, se o sequenciamento de porções dos adaptadores (“<i>adapter read-through</i>”) acontecer, irá ocorrer simultaneamente nos reads forward e reverse correspondentes a um mesmo fragmento de DNA na mesma posição. Além disso, como o fragmento terá sido sequenciado completamente nos dois sentidos, as porções dos reads forward e reverse que forem correspondentes à informação biológica serão reverso-complementares entre si. Nesse cenário, que qualquer base que estiver fora da região de complementaridade possivelmente é proveniente de um adaptador e pode ser removida. 
<br><br>
A abordagem palindrômica pode identificar tanto cenários em que praticamente não há informação válida nos reads (e uma boa porção dos adaptadores foi sequenciada, demonstrada no exemplo A da imagem abaixo), quanto situações em que apenas uma base do adaptador foi sequenciada, demonstrada no exemplo B. 
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_36.png" alt="Exemplos de adapter read-through detectados pelo Trimmomatic" align="center">
</center>
<br><br>
Como nesta abordagem os alinhamentos podem ser bem maiores dos alinhamentos produzidos no alinhamento da abordagem simples, o valor mínimo de score precisa ser maior para manter a especificidade do processo, próximo de 30. Mesmo que esse valor mínimo seja bem alto e exija pelo menos 50 matches ao longo da sequência, ele ainda permite que o Trimmomatic identifique fragmentos curtos de adaptadores. Esse valor mínimo é informado no segundo argumento do comando <i>ILLUMINACLIP</i>, e nesse exemplo usamos “<b>30</b>”, indicando que são necessários pelo menos 50 matches na região complementar do alinhamento para identificar um adaptador na região não-complementar.
</div>

### LEADING/TRAILING

<div align="justify">
Esses commandos removem bases de baixa qualidade do começo (<i>LEADING</i>) ou final (<i>TRAILING</i>) do read, de acordo com um valor mínimo especificado. Caso a primeira base apresente um valor abaixo do valor exigido ela será removida, e as próximas bases serão avaliadas sucessivamente, até que uma base com valor igual ou superior ao exigido seja encontrada. Nesse exemplo utilizamos o valor "<b>25</b>" no comando <i>LEADING</i>, informando ao Trimmomatic que bases no início dos reads que possuam qualidade inferior à 25 devem ser removidas.
</div>

### SLIDINGWINDOW

<div align="justify">
O comando <i>SLIDINGWINDOW</i> informa ao Trimmomatic para realizar a análise de qualidade dos reads ao longo de janelas, e cortar as bases somente quando a qualidade média dentro da janela analisada estiver abaixo de um valor mínimo determinado. Ao considerar várias bases ao mesmo tempo, o Trimmomatic garante que bases de qualidade não sejam removidas do conjunto por conta de apenas uma única base de qualidade inferior.  	
<br><br>
O primeiro argumento informa o tamanho da janela em número de bases (nesse exemplo, “<b>4</b>”) e o segundo argumento informa a qualidade média que é exigida para manter os dados (nesse exemplo, uma qualidade média de “<b>25</b>”).
</div>

### AVGQUAL

<div align="justify">
O comando <i>AVGQUAL</i> informa ao Trimommatic para remover reads que apresentem uma qualidade média (levando em consideração todas as bases) inferior a um valor especificado. Nesse exemplo utilizamos o valor “<b>25</b>”, informando ao Trimmomatic que reads de qualidade média inferior à 25 devem ser removidos.
</div>

### MINLEN

<div align="justify">
O comando <i>MINLEN</i> remove reads de tamanho inferior a um tamanho mínimo especificado, e geralmente deve ser utilizado após todos os passos de processamento, como uma etapa final. Nesse exemplo utilizamos o valor “<b>50</b>”, informando ao Trimmomatic que reads de tamanho inferior à 50 bases devem ser removidos.
<br><br>
Resumidamente, nosso processamento dos reads incluiu as seguintes etapas, nesta ordem:
<br><br>
<ul>
<li>Remoção dos adaptadores</li>
<li>Remoção de bases de qualidade baixa no início dos reads</li>
<li>Remoção de regiões de qualidade baixa ao longo dos reads de acordo com janelas específicas</li>
<li>Remoção de reads de qualidade baixa (considerando a qualidade média do read como um todo)</li>
<li>Remoção de reads muito curtos</li>
</ul>
</div>

## Filtragem e limpeza dos reads no Trimmomatic no Galaxy

<div align="justify">
A filtragem e limpeza dos reads também pode ser realizada no Galaxy, <a href="https://cursogenomicaegeneticaufpr.netlify.app/docs/praticas/aula_02/#primeira-avalia%C3%A7%C3%A3o-de-qualidade-no-fastqc-no-galaxy">após a análise da qualidade dos reads no FastQC</a>.
<br><br>
Para isso, na aba “<i>Tools</i>”, na região esquerda da página, digite “<i>Trimmomatic</i>” e selecione a segunda opção “<i>Trimmomatic: flexible read trimming tool for Illumina NGS data</i>”, para utilizar a versão mais recente do software.
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_37.png" alt="Ferramenta Trimmomatic na aba Tools do Galaxy" align="center">
</center>
<br><br>
Ao abrir a aba para seleção de arquivos e opções, selecione a opção “<i>Paired-end (two separate input files)</i>” no campo “<i>Single-end or paired-end reads?</i>”, informando que analisaremos reads pareados:
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_38.png" alt="Opções no campo Single-end or paired-end reads? da ferramenta Trimmomatic no Galaxy" align="center">
</center>
<br><br>
Em seguida, clique no ícone da opção “<i>Browse Datasets</i>” no campo “<i>Input FASTQ file (R1/first of pair)</i>” para selecionar o arquivo com reads forward para análise:
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_39.png" alt="Opção Browse Datasets para escolha de arquivos da ferramenta Trimmomatic no Galaxy" align="center">
</center>
<br><br>
Na janela seguinte, selecione o arquivo correspondente os reads forward (<b>SRR9672751_1</b>):
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_40.png" alt="Lista de arquivos dentro da aba Browse Datasets, com destaque para o arquivo SRR9672751_1" align="center">
</center>
<br><br>
Para incluir o arquivo com reads reverse na análise, clique novamente na opção “<i>Browse Datasets</i>” no campo “<i>Input FASTQ file (R2/second of pair)</i>”:
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_41.png" alt="Opção Browse Datasets para escolha de arquivos da ferramenta Trimmomatic no Galaxy" align="center">
</center>
<br><br>
Na janela seguinte, selecione o arquivo correspondente os reads reverse (<b>SRR9672751_2</b>):
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_42.png" alt="Lista de arquivos dentro da aba Browse Datasets, com destaque para o arquivo SRR9672751_2" align="center">
</center>
<br><br>
Agora iremos configurar o comando <i>ILLUMINACLIP</i>. Selecione a opção “<i>Yes</i>” no campo “<i>Perform initial ILLUMINACLIP step?</i>”. Em seguida, informe que deseja usar as sequências padrão do Trimmomatic, escolhendo a opção “<i>Standard</i>” no campo “<i>Select adapter sequences or provide custom?</i>”
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_43.png" alt="Configuração inicial da opção ILLUMINACLIP do Trimmomatic no Galaxy" align="center">
</center>
<br><br>
No campo “<i>Adapter sequences to use</i>” precisamos escolher o conjunto de sequências referente à estratégia de sequenciamento utilizada. Nesse caso, ao consultar a <a href="https://www.ncbi.nlm.nih.gov/sra/SRX6433203%5baccn%5d">página do depósito destes dados no SRA</a>, podemos determinar que a plataforma utilizada foi “<i>Illumina HiSeq</i>” e os dados são pareados. Sendo assim, a opção a ser escolhida é “<i>TruSeq3 (paired-ended, for MiSeq and HiSeq)</i>”:
<br><br>
Em seguida, iremos configurar os argumentos para o comando <i>ILLUMINACLIP</i> <a href="https://cursogenomicaegeneticaufpr.netlify.app/docs/praticas/aula_02/#filtragem-e-limpeza-dos-reads-no-trimmomatic">da mesma forma que configuramos para a linha de comando</a>:
<br><br>
<ul>
<li><b>Maximum mismatch count which will still allow a full match to be performed: </b>2 (quantos mismatches são aceitos na etapa de alinhamento “seed”)</li>
<li><b>How accurate the match between the two ‘adapter ligated reads must be for PE palindrome read alignment: </b>30 (qual o score de alinhamento mínimo para identificar um adaptador na abordagem palindrômica)</li>
<li><b>How accurate the match between any adapter etc. sequence must be against a read: </b>10 (qual o score mínimo de alinhamento para identificar um adaptador na abordagem de alinhamento simples)</li>
</ul>

As outras opções podem ser mantidas com a configuração padrão do Galaxy:
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_44.png" alt="Configuração de opções do ILLUMINACLIP do Trimmomatic no Galaxy" align="center">
</center>
<br><br>
Na seção “<i>Trimmomatic Operation</i>” iremos adicionar as etapas de filtragem e limpeza a serem realizadas na mesma ordem que realizamos para a linha de comando. A primeira etapa após o comando <i>ILLUMINACLIP</i> era do comando “<i>LEADING</i>”, para remover bases de baixa qualidade no início dos reads. Selecione a opção “<i>Cut bases off the start of a read, if below a threshold quality (LEADING)</i>” no campo “<i>Select Trimmomatic operation to perform</i>” e informe o valor de qualidade “<i>25</i>” no campo “<i>Minimum quality required to keep a base</i>” para remover bases de qualidade inferior à 25, e clique em “<i>Insert Trimmomatic Operation</i>” para adicionar mais uma etapa:
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_45.png" alt="Configuração da opção LEADING do Trimmomatic no Galaxy" align="center">
</center>
<br><br>
Agora iremos adicionar a etapa do comando “<i>SLIDINGWINDOW</i>”, para avaliar a qualidade dos reads ao longo de janelas. Selecione a opção “<i>Sliding window trimming (SLIDINGWINDOW)</i>” no campo “<i>Select Trimmomatic operation to perform</i>”, informe o tamanho da janela como "<b>4</b>" no campo “<i>Number of bases to average across</i>”, e o valor de qualidade como “<b>25</b>” no campo “<i>Average quality required</i>”. Em seguida, clique novamente em “<i>Insert Trimmomatic Operation</i>” para adicionar uma terceira etapa:
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_46.png" alt="Configuração da opção SLIDINGWINDOW do Trimmomatic no Galaxy" align="center">
</center>
<br><br>
A terceira etapa será a do comando “<i>AVGQUAL</i>”, para manter reads com uma qualidade média de acordo com um valor mínimo especificado. Selecione a opção “<i>Drop reads with average quality lower than a specified level (AVGQUAL)</i>” no campo “<i>Select Trimmomatic operation to perform</i>” e informe o valor de qualidade de “<b>25</b>” no campo “<i>Mininum average quality required to keep a read</i>”. Clique novamente em “<i>Insert Trimmomatic Operation</i>” para adicionar a última etapa:
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_47.png" alt="Configuração da opção AVGQUAL do Trimmomatic no Galaxy" align="center">
</center>
<br><br>
Agora seguiremos para a última etapa com o comando “<i>MINLEN</i>”, removendo reads de tamanho inferior à um valor especificado. Selecione a opção “<i>Drop reads below a specified length (MINLEN)</i>” no campo “<i>Select Trimmomatic operation to perform</i>” e informe o tamanho de “<b>50</b>” no campo “<i>Mininum length of reads to be kept</i>”:
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_48.png" alt="Configuração da opção MINLEN do Trimmomatic no Galaxy" align="center">
</center>
<br><br>
Por fim, selecione a opção “<i>Yes</i>” nos campos “<i>Output trimmlog file?</i>” e “<i>Output trimmomatic log messages</i>” para criar os arquivos de log do processo, e clique em “<i>Execute</i>” para iniciar a filtragem e limpeza dos arquivos:
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_49.png" alt="Configuração final do Trimmomatic no Galaxy" align="center">
</center>
<br><br>
Quando a filtragem e processamento de ambos os arquivos estiverem concluídas, os seis arquivos estarão listados em verde.  Podemos avaliar o arquivo log para verificar se o processamento ocorreu corretamente e quantos reads foram eliminados. Para isso, baixe o arquivo sob o nome “<b>Trimmomatic on data (log file)</b>” utilizando a opção “<i>Download</i>”: 
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_50.png" alt="Análise do Trimmomatic finalizada no Galaxy, indicada pelos arquivos listados em verde, e opção de download do arquivo LOG" align="center">
</center>
<br><br>
Abra o arquivo em algum editor de texto simples (como o Notepad ++). Nas linhas iniciais há algumas informações sobre o software, o comando utilizado e a sequência dos adaptadores utilizada na busca. Na linha 9 encontramos informações sobre os reads que foram mantidos e removidos:
<br><br>
<ul>
<li><b>Input Read Pairs: </b>quantidade de reads considerando os dois arquivos de entrada (20.773.714 reads)</li>
<li><b>Both Surviving: </b>reads que foram mantidos nos arquivos forward e reverse após a filtragem (18.377.330 reads, 88,46% dos reads de entrada, divididos entre os arquivos FASTQ de saída “<b>R1 paired</b>” e “<b>R2 paired</b>”)</li>
<li><b>Forward Only Surviving: </b>reads que foram mantidos no arquivo forward, mas cujo par não foi mantido no arquivo reverse após a filtragem (1.099.676 reads, 5,29% dos reads de entrada, presentes no arquivo FASTQ de saída “<b>R1 unpaired</b>”)</li>
<li><b>Reverse Only Surviving: </b>reads que foram mantidos no arquivo reverse, mas cujo par não foi mantido no arquivo reverse após a filtragem (651.542 reads, 3,14% dos reads de entrada, presentes no arquivo FASTQ de saída “<b>R2 unpaired</b>”)</li>
<li><b>Dropped: </b>reads que foram removidos por não atenderem aos critérios exigidos nas etapas de filtragem (645.166 reads, 3,11% dos reads de entrada, não são salvos em nenhum arquivo)</li>
</ul>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_51.png" alt="Arquivo LOG do Trimmomatic" align="center">
</center>
<br><br>
Podemos perceber que boa parte dos pares de reads sobreviveu ao processo de filtragem (88.46%), o que já era esperado considerando que a primeira avaliação dos arquivos no FastQC atestava a boa qualidade do conjunto de dados. Entretanto, apenas olhando o arquivo LOG do Trimmomatic não é possível perceber se os adaptadores foram totalmente removidos e se podemos prosseguir com a montagem do genoma. Sendo assim, avaliaremos os arquivos filtrados com o FastQC, seguindo a mesma lógica de avaliação dos arquivos originais.  
<br><br>
</div>

## Segunda avaliação de qualidade no FastQC

<div align="justify">
Com os reads processados pelo Trimmomatic em mãos, iremos avaliar se a limpeza e processamento foram suficientes, e se a qualidade, comprimento, quantidade dos reads é adequada, e se os adaptadores foram removidos utilizando novamente o software FastQC. Os processos são os mesmos realizados para a primeira avaliação, seja <a href="https://cursogenomicaegeneticaufpr.netlify.app/docs/praticas/aula_02/#primeira-avalia%C3%A7%C3%A3o-de-qualidade-no-fastqc"utilizando o comando</a> ou <a href="https://cursogenomicaegeneticaufpr.netlify.app/docs/praticas/aula_02/#primeira-avalia%C3%A7%C3%A3o-de-qualidade-no-fastqc-no-galaxy">no Galaxy</a>, mas desta vez usando os <a href="https://cursogenomicaegeneticaufpr.netlify.app/docs/praticas/aula_02/#filtragem-e-limpeza-dos-reads-no-trimmomatic">arquivos FASTQ filtrados pelo Trimmomatic</a> (<b>R1 paired, R2 paired, R1 unpaired e R2 unpaired</b>). 
<br><br>
<a href="https://cursogenomicaegeneticaufpr.netlify.app/docs/praticas/aula_02/#primeira-avalia%C3%A7%C3%A3o-de-qualidade-no-fastqc">A interpretação dos módulos de resultados do arquivo HTML é a mesma descrita anteriormente</a>. Como pontos importantes a ter em mente nessa segunda avaliação, destacamos os seguintes:
<br><br>
</div>

### Posições de baixa qualidade nos reads foram removidas?

<div align="justify">
Para avaliar esse ponto, podemos comparar os resultados do módulo “<a href="https://cursogenomicaegeneticaufpr.netlify.app/docs/praticas/aula_02/#per-base-sequence-quality-qualidade-da-sequ%C3%AAncia-em-cada-uma-das-bases"><i>Per base sequence quality</i></a>”. Podemos ver que para todos os arquivos (<b>R1 paired, R2 paired, R1 unpaired e R2 unpaired</b>) ocorreu a remoção de posições de qualidade reduzida ao longo dos reads, se comparados aos arquivos originais. Neste exemplo abaixo, é possível perceber que a distribuição de qualidade para cada posição é diferente após a filtragem:
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_52.png" alt="Comparação de resultados do módulo Per base sequence quality antes e depois da filtragem" align="center">
</center>
<br><br>
</div>

### Reads de baixa qualidade foram removidos?

<div align="justify">
Para avaliar este ponto, podemos comparar os resultados do módulo “<a href="https://cursogenomicaegeneticaufpr.netlify.app/docs/praticas/aula_02/#per-sequence-quality-scores-qualidade-por-sequ%C3%AAncia-read"><i>Per sequence quality score</i></a>”. Podemos ver que para todos os arquivos (<b>R1 paired, R2 paired, R1 unpaired e R2 unpaired</b>) ocorreu a remoção de reads com qualidade baixa, se comparados aos arquivos originais. Neste exemplo abaixo, é possível observar que a distribuição é diferente após a filtragem, iniciando em 28, pois reads de qualidade abaixo de 25 foram removidos:
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_53.png" alt="Comparação de resultados do módulo Per sequence quality scores antes e depois da filtragem" align="center">
</center>
<br><br>
</div>

### Os adaptadores foram removidos?

<div align="justify">
Para avaliar este ponto, podemos comparar os resultados dos módulos “<a href="https://cursogenomicaegeneticaufpr.netlify.app/docs/praticas/aula_02/#adapter-content-presen%C3%A7a-de-adaptadores"><i>Adapter Content</i></a>” e “<a href="https://cursogenomicaegeneticaufpr.netlify.app/docs/praticas/aula_02/#overrepresented-sequences-sequ%C3%AAncias-super-representadas"><i>Overrepresented sequences</i></a>”. Podemos ver que para quase todos os arquivos (<b>R1 paired, R2 paired e R2 unpaired</b>) ocorreu a remoção das sequências de adaptadores, se comparados aos arquivos originais. Neste exemplo abaixo, no módulo “<i>Adapter Content</i>” é possível que a distribuição é diferente após a filtragem, e as sequências de adaptadores estão ausentes:
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_54.png" alt="Comparação de resultados do módulo Adapter content antes e depois da filtragem" align="center">
</center>
<br><br>
Além disso, sequências de adaptadores que eram listadas no módulo “<i>Overrepresented sequences</i>” estão ausentes:
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_55.png" alt="Comparação de resultados do módulo Overrepresented sequences antes e depois da filtragem" align="center">
</center>
<br><br>
Para o arquivo “<b>R1 unpaired</b>”, entretanto, as sequências de adaptadores não foram totalmente removidas pelo Trimmomatic, sendo identificadas pelo módulo “<i>Overrepresented sequences</i>”:
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_56.png" alt="Comparação de resultados do módulo Overrepresented sequences antes e depois da filtragem" align="center">
</center>
<br><br>
Para o arquivo “<b>R1 unpaired</b>”, entretanto, as sequências de adaptadores não foram totalmente removidas pelo Trimmomatic, sendo identificadas pelo módulo “<i>Overrepresented sequences</i>”:
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_57.png" alt="Erros no módulo Overrepresented sequences, com várias sequências de adaptadores detectadas" align="center">
</center>
<br><br>
No contexto das atividades práticas do curso isso não será um problema, pois utilizaremos apenas os dados pareados para realizar a montagem do genoma e desconsideraremos os arquivos “<b>R1 unpaired</b>” e “<b>R2 unpaired</b>”. Caso fosse necessário utilizar os arquivos não pareados por alguma razão, as possibilidades de solução deste problema seriam:
<br><br>
<ul>
<li>Repetir a filtragem do arquivo original, alterando alguns parâmetros e editando o arquivo de adaptadores para incluir estas sequências que não foram reconhecidas pelo Trimmomatic</li>
<li>Filtrar o arquivo “<b>R1 unpaired</b>” até remover totalmente as sequências de adaptadores, possivelmente editando o arquivo de adaptadores para incluir as sequências que não foram reconhecidas na primeira filtragem</li>
</ul>
</div>

### Algum outro módulo apresentou algum aviso ou erro significativo?

<div align="justify">
Podemos ver que para quase todos os arquivos (<b>R1 paired, R2 paired e R2 unpaired</b>) foram gerados avisos nos módulos “<a href="https://cursogenomicaegeneticaufpr.netlify.app/docs/praticas/aula_02/#per-base-sequence-content-conte%C3%BAdo-de-sequ%C3%AAncia-em-cada-uma-das-bases"><i>Per base sequence content</i></a>”, “<a href="https://cursogenomicaegeneticaufpr.netlify.app/docs/praticas/aula_02/#per-sequence-gc-content-conte%C3%BAdo-gc-por-sequ%C3%AAncia"><i>Per sequence GC content</i></a>” e “<a href="https://cursogenomicaegeneticaufpr.netlify.app/docs/praticas/aula_02/#sequence-length-distribution-distribui%C3%A7%C3%A3o-de-tamanho-das-sequ%C3%AAncias"><i>Sequence length distribution</i></a>”. Como já discutimos anteriormente na avaliação dos arquivos originais, os avisos que surgiram para estes módulos possivelmente estão associados a questões biológicas da própria amostra, e não sugerem problemas. Entretanto, é sempre importante relembrar que dependendo do contexto estes avisos podem indicar a existência de problemas a serem resolvidos, e sempre devem ser avaliados com atenção.
<br><br>
Sendo assim, agora que nos certificamos de que o processamento e filtragem dos dados foi adequado para os arquivos forward e reverse, prosseguiremos para a próxima atividade prática de montagem de genomas.	
</div>


## Aula em vídeo

<br>
<div align="center">
<iframe width="560" height="315" src="https://www.youtube.com/embed/ frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> 
<br><br>
Clique <a href="https://photos.app.goo.gl/">aqui</a> para fazer o download do vídeo.
<br><br>
</div>