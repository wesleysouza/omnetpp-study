# OMNeT++

## Criando um projeto:

File -> New -> OMNeT++ Project  -> Nome do Projeto -> Empty Project -> Finish

### Criando Modulo

New Simple Module -> Defina um Nome -> Next -> A Simple Module

Editamos o módulo pela aba design ou source. Em source podemos fazer definições via código C++. Em design alteramos a aparência e etc.

Definindo parâmetros no source:

```cpp
simple Dispositivo
{
    parameters:
        bool mandarPrimeiraMesagem = default(true); //Vai enviar a primeira mensagem
        int limite = default(10); //Limite de mensagens enviadas
    @display("i=device/laptop")
    //Definindo as portas de comunicação
    gates:
        input in;
        output out;
}
```

No momento da criação do Modulo também foram criados os seguintes arquivos:
- **dispositivo.h**: interface de programação para o dispositivo;
- **dispositivo.cc**: implementação em C++ do **dispositivo.h**;

Em **dispositivo.h** vamos definir os protótipos de nossas funções:

```cpp
class Dispositivo : public cSimpleModule
{
    protected:
        short timeToLive; // Numero maximo de mensagens que podem ser enviadas.
        virtual void initialize();
        virtual void handleMessage(cMessage *msg);
}
```

Em **dispositivo.cc** podemos definir as funcionalidades, implementar os protótipos das funções que estão em **dispositivo.h**.

```cpp
void Dispositivo::initialize()
{
    timeToLive = par("limite"); // Capturar informações sobre o limite;
    // Verifique se esse sispositivo está configurado para enviar a primeira mensagem
    if (par("mandarPrimeiraMensagem"), boolValue() == true){
        EV << getName << ".Mensagem Transmitida " << "Mensagens que esse host ainda pode receber" << timeToLive << ".\n";
        cMessage *msg = new cMessage("Mensagem Transmitida");
        send(msg, "out"); // Enviando a mensagem para a porta de saída padrão "out" do dispositivo
    }
}

void Dispositivo::handleMessage(cMessage *msg)
{
    WATCH(timeToLive); //Adição de informação visual mostrando quantas vezes eu posso transmitir mensagens em cada dispositivo
    if (timeToLive){
        bubble("A mensagem foi Recebida");
        msg = new cMessage("Mensagem retransmitida");
        EV << "Nome: " << getNome() << ". Mensagens que esse host ainda pode receber" << timeToLive << ".\n";
        send(msg, "out");
        timeToLive--;
        getDisplayString().setTagArg("t", 0, timeToLive);
    }
    else
    {
        delete msg;
        EV << "Nome: " << getNome() << ". Fim da Transmissão. A mensagem residual foi destruída. TTL alcançado" << ".\n";
    }
}
```

Comandos importantes:
- **EV**: Gera um relatório da simulação em tempo de execução. Funciona semelhante ao cinin e cout na linguagem C++ nativa.
- **cMessage**: Objeto do OMNeT.
- **bubble**: Palavra reservada para envio de informação em formato bolha.

### Implementando uma rede

Ambiente no qual o nosso dispositivo vai atuar.

New Network -> An empty Network.

A rede também vai ter as abas de design e source. É possível adicionar os abjetos criados na rede e adicionar conexões nos dispositivos.

Na aba source poemos definir configurações adicionais como o **delay de transmissão** em cada conexão.

```cpp
network Rede
{
    @display("...");
    submodules:
        dispositivo: Dispositivo {
            @display("...");
            mandarPrimeiraMensagem = true;
        dispositivo1: Dispositivo {
            @display("...")
            mandarPrimeiraMensagem = false;
        }
    connections:
    dispositivo.out --> { delay = 1000ms; } --> dispositivo1.in;
    dispositivo1.out --> { delay = 1000ms; } --> dispositivo.in;
}
```

### Executando o projeto

Execute a rede como OMNeT++ Simulation, ele vai pedir para criar um ini file, aceite. Além disso, clique ok na configuração de simulação.