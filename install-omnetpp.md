# Instalando o Omnet++ no Ubuntu 20.04

## Passo 1: Update e Upgrade

Abra o terminal e digite o comando abaixo:
```
sudo apt update && sudo apt upgrade -y
```

## Passo 2: Instalando os pacotes

Na documentação do Omnet++ ele recomenda a instalação dos pacotes abaixo:

```
sudo apt-get install build-essential gcc g++ bison flex perl python python3 qt5-default libqt5opengl5-dev tcl-dev tk-dev libxml2-dev zlib1g-dev default-jre doxygen graphviz
```

Além desses, a documentação também recomenda a instalação do pacote libwebkitgtk-3.0-0. No entando, se você tentar instalar esse pacote no Ubuntu 20.04 vai ter como resultado o erro abaixo:

The Package 'libwebkitgtk-3.0-0' has no installation candidate. 

Para instalar o libwebkitgtk-3.0-0 precisamos adicionar um respositório do Ubuntu 16.04 no 20.04.

### Instalando o libwebkitgtk-3.0-0 no Ubuntu 20.04

Abra o arquivo **/etc/apt/sources.list** com o nano ou o vim.

Adicione a linha abaixo no final do arquivo:

```
deb http://cz.archive.ubuntu.com/ubuntu bionic main universe
```

Atualize o sistema com o comando abaixo:

```
sudo apt update
```

Agora podemos instalar o libwebkitgtk-3.0-0, instale com o comando abaixo:

```
sudo apt-get install libwebkitgtk-3.0-0
```

### Pacotes Extras

MPI Install

```
sudo apt-get install openmpi-bin libopenmpi-dev
```

#PCAP Install 

```
sudo apt-get install libpcap-dev
```

## Passo 3: Baixando e Instalando o Omnetpp

É possível baixar o Omnetpp nesse link (Omnetpp)[http://omnetpp.org], nesse tutorial vamos usar a versão 5.6.2.

## Passo 4: Unpacking (Descompactado)

Para descompactar use o comando abaixo:

```
tar xvfz omnetpp-5.6.2-src.tgz
```

## Passo 5: Configurando as variáveis de ambiente

Para iniciar a configuração digite os comandos abaixo:

```
cd omnetpp-5.6.2
. setenv
```

Com o comando **. setenv** você vai saber o caminho da pasta do Omnetpp no seu sistema para exportar para a variável de ambiente. Para fazer isso abra o arquivo **bashrc** com o comando abaixo:

```
gedit ~/.bashrc
```

Adicione a linha abaixo no arquivo, verifique se o caminho do Omnetpp está correto.

```
export PATH=$HOME/omnetpp-5.6.2/bin:$PATH
```

Após fazer isso, abra e feche o terminal para que as modificações tenham efeito.

## Passo 6: Configurando e Construindo

No nível mais alto do diretório do seu OMNeT++ execute o comando:

```
./configure
```

Caso você for utilizar o OMNeT++ por meio de uma sessão ssh configure com o comando abaixo:

```
./configure WITH_TKENV=no WITH_QTENV=no
```

Finalmente, podemos construir o OMNeT++ no nosso sistema.

```
make
```

## Passo 7: Verificando instalação:

```
cd samples/dyna
./dyna
```

## Passo 8: Starting IDE

No terminal, digite o comando:

```
omnetpp
```

## Extra

É possível configurar ícones para o OMNeT++ utilize os comandos abaixo:

```
make install-menu-item
make install-desktop-icon
```



