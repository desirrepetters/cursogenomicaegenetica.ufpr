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
<br><br>
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
<br><br>
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
<br><br>
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
<br><br>
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
<br><br>
</div>

## Sequence duplication levels (Nível de duplicação das sequências)

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
<ul>
<li>Em bibliotecas de RNA-Seq, diferentes transcritos apresentam diferentes níveis de expressão, e possivelmente os transcritos mais frequentes serão sequenciados muito mais vezes que transcritos não tão frequentes</li>
<li>Se o sequenciamento está sendo realizado com uma cobertura bastante alta, à medida que todas as regiões do genoma forem sequenciadas, é provável que algumas regiões sejam sequenciadas vária vezes. Nesse tipo de cenário, encontrar muitas sequências duplicadas pode sugerir que a cobertura do sequenciamento não precisava ser tão alta</li>
</ul>
<br><br>
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
<br><br>
</div>

## Overrepresented sequences (Sequências super representadas)

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
Nesse tipo de situação, é importante <a href="https://cursogenomicaegeneticaufpr.netlify.app/docs/praticas/aula_02/#filtragem-e-limpeza-dos-reads-no-trimmomatic">realizar o processamento e filtragem do arquivo e a remoção de adaptadores</a>, e <a href="https://cursogenomicaegeneticaufpr.netlify.app/docs/praticas/aula_02/segunda-avaliacao-de-qualidade-no-fastqc">avaliar se as sequências listadas deixaram de existir</a>. Se ainda persistirem, é interessante investigar a possível identidade das sequências por meio do BLAST, ou seguir para análises posteriores e investigar a contaminação em outros momentos (como na avaliação de montagem de genomas). Por outro lado, dependendo do contexto e do tipo de experimento, estas sequências podem representar sequências reais que realmente estavam mais presentes na amostra que outras. Aqui também reforçamos a importância de lembrar do contexto do experimento ao avaliar os resultados deste módulo.
<br><br>
Um outro tipo de situação problemática ocorre quando todas as sequências super representadas representam sequências de adaptadores, e em alta quantidade, como neste último exemplo. Neste caso, o FastQC conseguiu atribuir a identidade da maior parte dessas sequências, caracterizando-as como adaptadores Illumina. Além disso, elas representam uma fração significativa dos dados, em frequências de 8,12%, 5,08%, 1,08%. Esse tipo de observação sugere que a há contaminação com dímeros de adaptadores, e pode ser necessário sequenciar o material novamente.
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/praticas/img/aula_02/aula_02_27.png" alt="Contaminação de dímeros de adaptadores detectada no módulo Overrepresented sequences do FastQC" align="center">
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

## Segunda avaliação de qualidade no FastQC

## Aula em vídeo

<br>
<div align="center">
<iframe width="560" height="315" src="https://www.youtube.com/embed/yz0njYN0oIw" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> 
<br><br>
Clique <a href="https://photos.app.goo.gl/pukRgyGobNrkH3Un7">aqui</a> para fazer o download do vídeo.
<br><br>
</div>