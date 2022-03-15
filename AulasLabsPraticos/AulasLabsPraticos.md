# Aulas de Laboratórios Práticos


1. Laboratório 1: Routing I (com máquinas virtuais) 
2. Laboratório 2: Routing II (com NAT e acesso à Internet)
3. Laboratório 3: Captura de Tráfego numa Rede  

## 0. Preparação dos Laboratórios / Pré-Requisitos

### 0.1 Instalação de Máquinas Virtuais em equipamentos dos alunos (Labs 1,2 e 3)

***Pré-requisito: VirtualBox instalado em Windows, MacOS, Linux.***

Para as aulas práticas iremos utilizar os materiais do ***[SEED Labs Project](https://seedsecuritylabs.org/)***, mais precisamente a máquina virtual ***SEEDUbuntu16.04***.
Esta [página](https://seedsecuritylabs.org/labsetup.html) na secçãoo Ubuntu 16.04 estão vários links para download (pode fazer a partir [daqui](https://drive.google.com/file/d/12l8OO3PXHjUsf9vfjkAf7-I6bsixvMUa/view?usp=sharing)) da imagem e do [manual de instalação](https://seedsecuritylabs.org/Labs_16.04/Documents/SEEDVM_VirtualBoxManual.pdf) para o VirtualBox.

Instruções resumidas (podem não dispensar a consulta do manual referido anteriormente):

1. Efetuar o download da imagem da máquina virtual em Linux/Ubuntu 16.04
2. Criar uma Máquina Linux no VirtualBox com o nome que pretender, sugere-se UE01. Irá necessitará de criar várias máquinas.

Após escolher a opção criar máquina virtual deve 1) dar nome à VM, 2) escolher o sistema operativo (para o caso da máquina SEEDUbuntu-16-04-32bit poderá escolher 32bits), 3) diminuir a memória para 512 MB e 4) escolher a opção de **não** adicionar nenhum disco virtual (adicionará seguidamente o ficheiro que fez download).

<img src="seed-ubuntu-16-04-fig1.png" alt="Desktop no PC" width="600"/>

3. Descomprimir o ficheiro .zip que fez download para o mesmo diretorio da máquina virtual e adicionar o disco à VM UE01.

4. Após criar a máquina deverá entrar nos *settings* da máquina, entrar na *tab* *Storage* e escolher a opção de adicionar um disco.

<img src="seed-ubuntu-16-04-fig2.png" alt="Desktop no PC" width="600"/>

Deve escolher o ficheiro principal do disco da máquina.

<img src="seed-ubuntu-16-04-fig3.png" alt="Desktop no PC" width="400"/>

Após adicionar, o disco deverá aparecer nas configuração de *Storage* da máquina virtual.

<img src="seed-ubuntu-16-04-fig4.png" alt="Desktop no PC" width="600"/>

5. Poderá ter de escolher a placa gráfica VBoxVGA
6. Iniciar a máquina virtual (credenciais - user:seed; password:dees)
7. Atualizar o sistema operativo: sudo apt update; sudo apt upgrade
8. Atualizar o teclado para português: setxkbmap pt

*Nota: É possível correr as máquinas dos SEED LABS na Cloud. [Neste repositório](https://github.com/seed-labs/seed-labs/blob/master/manuals/cloud/seedvm-cloud.md) da SEED Labs poderá verificar como o fazer. Não haverá suporte do Docente para esta configuração*

# Laboratórios
Para a calendarização prevista dos seguintes laboratórios ver as [aqui](https://github.com/pmrosa-classes/RedesComputadoresIG/blob/main/README.md#planeamento-previsto-pode-sofrer-alteraçõesplaneamento) as datas.

## 1. Laboratório 1: Routing I 

Seguir os [seguintes](https://github.com/pmrosa-classes/RedesComputadoresIG/blob/main/AulasLabsPraticos/RoutingI.md) passos para a realização do laboratório.

## 2. Laboratório 2: Routing II

Seguir os [seguintes](https://github.com/pmrosa-classes/RedesComputadoresIG/blob/main/AulasLabsPraticos/RoutingII.md) passos para a realização do laboratório.

## 3. Laboratório 3: Captura de Tráfego numa Rede 

Seguir os [seguintes](https://github.com/pmrosa-classes/RedesComputadoresIG/blob/main/AulasLabsPraticos/CapturaTrafegoRede.md) passos para a realização do laboratório.

