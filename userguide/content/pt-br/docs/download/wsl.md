---
title: "Ambiente Linux: Windows Subsystem for Linux (WSL)"
linkTitle: "Ambiente Linux: Windows Subsystem for Linux (WSL)"
weight: 5
description: >
  Como instalar o ambiente Linux em um sistema Windows 10
---
<div align="justify">
O subsistema do Windows para Linux é um módulo disponível para o Windows 10 que permite que os usuários utilizem um ambiente GNU/Linux com a maioria das ferramentas de linha de comando, utilitários e aplicativos diretamente no Windows, evitando problemas, modificações e sobrecarga normalmente observados ao utilizar softwares de máquinas virtuais tradicionais ou uma instalação dualboot.
<br><br>
Muitas das ferramentas de estudos genômicos são feitas apenas para sistemas Linux, sem versões funcionais em Windows, fazendo com que o acesso a um sistema Linux seja essencial. Além disso, muitas ferramentas de processamento por linha de comando presentes no Linux são extremamente úteis para processar os arquivos de saída das análises. Nesse sentido, possuir o ambiente Linux diretamente associado ao sistema Windows é bastante conveniente. 
<br><br>
No contexto do curso utilizaremos o WSL para realizar o processamento de alguns arquivos de saída e preparação de figuras, e também para algumas análises mais simples que não estão disponíveis no Galaxy. Nesta seção, descreveremos como preparar o Windows para receber o subsistema Linux, e como realizar a instalação da distribuição Ubuntu.
</div>

## Preparação

<div align="justify">
Para instalar qualquer distribuição do Linux (Ubuntu, Debian, entre outras), primeiro devemos habilitar o recurso opcional “<i>Subsistema do Windows para Linux</i>”. Para isso, procure o “<i>Windows Power Shell</i>” no menu “<i>Iniciar</i>”, clique em “<i>Executar como administrador</i>”, e na janela com a pergunta “<i>Deseja permitir que este aplicativo faça alterações no seu dispositivo?</i>” escolha a opção “<i>Sim</i>”:
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/download/img/wsl/wsl_1.png" alt="Execução do Windows PowerShell como administrador" align="center">
</center>
<br><br>
Após abrir o Windows Power Shell, digite o seguinte comando e aperte “<i>Enter</i>”:
<br><br>
</div>

```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

<div align="justify">
Em seguida, precisaremos habilitar o recurso opcional “<i>Plataforma de Máquina Virtual</i>”. Abra o Windows PowerShell novamente como administrador e execute o seguinte comando:
<br><br>
</div>

```
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

<div align="justify">
Após habilitar a “<i>Plataforma de Máquina Virtual</i>”, baixe o pacote de atualização do kernel do Linux do WSL2 clicando <a href="https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi">aqui</a>. Após baixar o arquivo, execute-o e selecione “<i>sim</i>” para aprovar a instalação da atualização. Ao terminar a instalação, abra novamente o Windows PowerShell e configure a versão 2 do WSL como a versão padrão ao instalar uma distribuição Linux utilizando o seguinte comando:
<br><br>
</div>

```
wsl --set-default-version 2
```

## Instalação

<div align="justify">
Agora que o Windows está preparado para receber e rodar o sistema Linux, abra a <a href="https://aka.ms/wslstore">Microsoft Store</a> e escolha sua distribuição de preferência. No contexto do curso utilizaremos a distribuição Ubuntu:
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/download/img/wsl/wsl_2.png" alt="Distribuições Linux para o WSL disponíveis na Microsoft Store" align="center">
</center>
<br><br>
Na página da distribuição escolhida, clique em “Obter”:
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/download/img/wsl/wsl_3.png" alt="Opção Obter na distribuição Ubuntu na Microsoft Store" align="center">
</center>
<br><br>
Ao abrir a distribuição após a instalação pela primeira vez, uma janela de console será aberta e levará alguns minutos até que os arquivos da instalação sejam descompactados e armazenados no computador. Nas futuras inicializações esse tempo será bastante reduzido e o sistema estará pronto para uso rapidamente.
<br><br>
Outro ponto importante a ser mencionado é que na primeira vez em que a distribuição for aberta será necessário criar uma conta de usuário e senha. Em “<i>Enter new UNIX username</i>” escolha um nome de usuário e aperte “<i>Enter</i>”. Em seguida, escolha uma senha e aperte “<i>Enter</i>”. Por mais que a senha não apareça na tela ao ser digitada, ela terá sido definida. 
<br><br>
Para abrir a distribuição futuramente você pode procurá-la pelo nome no menu Iniciar:
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/download/img/wsl/wsl_4.png" alt="Distribuição Ubuntu no Menu Iniciar" align="center">
</center>
<br><br>
Além disso, uma janela de terminal do sistema Linux pode ser aberta dentro de qualquer pasta no Windows Explorer, a qualquer momento. Basta segurar a tecla “<i>Shift</i>” do teclado e clicar com o botão direito do mouse em uma área vazia da pasta de interesse, e escolher a opção “<i>Abrir o shell do Linux aqui</i>”:
<br><br>
<center>
<img src="https://raw.githubusercontent.com/desirrepetters/cursogenomicaegenetica.ufpr/master/userguide/content/pt-br/docs/download/img/wsl/wsl_5.png" alt="Opção Abrir o shell do Linux aqui no Windows" align="center">
</center>
<br><br>
</div>

