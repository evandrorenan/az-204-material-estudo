AZURE FUNCTIONS DEBUGGING

A opção _enable streaming_ permite consultar os logs da aplicação praticamente em tempo real

Existem duas formas de visualizar os lgs.

- _Built-in log streaming_ - Exibe o arquivo de logs da aplicação (streaming). 
- _Live Metric Stream_ - Pode ser utilizado quando a Function App está conectada com o Application Insights. Permite visualizar logs e outras métrica no Azure Portal usando _Live Metric Stream_. (O que é Live Metric Stream?)

Logs podem ser visualizados tanto no Portal quanto no ambiente local. A segunda opção é um pouco mais complexa de configurar.

AZURE FUNCTIONS - PRINCIPAIS CARACTERISTICAS

- Leves e serverless
- Fáceis de se criar e deployar
- Rápidas para executar por não estar contida em uma larga aplicação, não tempo de inicialização e outros eventos que costumam ser executados antes do código.
- São disparadas por um evento
- Não exigem definiçõe sde infra e não exigem manutenção.
- Podem ser buildadas, testadas e executadas no Azure Portal via browser
- Fáceis de atualizar e não afetam outras partes da aplicação.
- Usa protocolos padrões de mercado e podem se comunicar com outras APIs, bancos de dados e bibliotecas
- Só se paga pelo período em que as funções estão executando
- São auto escaláveis
- Podem escalar iinclusive, ate zero.
- Azure está integrado com Azure Monitor
- Possuem CI/CD disponíveis via Azure DevOps
- São disparadas por um trigger associado a um evento por evento.


CASOS DE USO

O ideal é usar em aplicações menores com eventos que podem funcionar independentemente de outros eventos da aplicação.

_Casos de uso de negócio_
- Tarefas agendadas
- Notificações
- Aplicações leves
- Enviar e-mails

_Casos de uso técnicos
- Tarefas de saúde do BD
- Filas e Mensageria
- Processamento de dados de IOT
- Processos batch


RUNTIMES

É o ambiente de computação connfigurado com o software necessário para executar uma aplicação.

Azure e compatível com os principais runtimes do mercado

- .NET
- Node.js
- Python
- Java
- Powershell Core
- Custom Handler - É difícil de fazer.

(Os runtimes são Docker Containers - disponíveis no Docker Hub)

WINDOWS VS LINUX hosting

Em geral o ideal na Azure é trabalhar com Windows. Todo o ecossistema é baseado nele.
Um exemplo de limitação: Não é possível editar uma função linux no Portal após ela já ter sido deployada.
Na maioria das vezes não dá pra perceber a diferença entre as duas opções.

TEMPLATES

Azure provê templates para os cenários mais comuns.
Inclusive via vscode.

Tipos de templates disponíveis (Triggered by):

HTTP
Timer
Blog Storage
Cosmos DB
Queue Storage
Event Grid - Disparado por outros serviços da Azure
Event Hub
Service bus Queue
Service bus Topics
SendGrid - (email)

CONFIGURAÇÃO DE UMA FUNÇÃO

As configurações das funções são definidas no arquivo function.json.
Nele são definidos os triggers, bindings e demais configurações.

Exemplo:

{ 
  "disabled": false,
  "bindings": [
    {
      "type": "bindingType",
      "direction":"in",
      "name": "myParamName"
    }
  ]
}

type = nome do binding
direction = indica se o binding é de entrada ou saída
name = usado pra identificar o parâmetro na função //confirmar


CONFIGURAÇÃO DO HOST

Toda Function App tem um arquivo de configuração chamado hosts.json

Esse arquivo de configurações contém as configurações globais para todas as funções da Function App.

PLAN SERVICES

Azure possui três tipos de planos disponíveis:

_Consumption Plan_ (Serverless) - Cold Starts
- Cobrança somente enquanto a função está sendo processada
- Cobrança baseada na quantidade de execuções, o tempo de cada execução, e a quantidade de memória utilizada
- Escala até 0


_Premium Plan_ (Serverless) - Warmed instances
- Processo de start da aplicação é executado previamente, assim o retorno é imediato

_Dedicated Plan_ - Disponível via App Service
- Se você criou um App Service, pode usar as mesmas VMs instanciadas para executar as funções, sem custo adicional.

TRIGGERS E BINDINGS

Abstraem eventos, entradas e saídas eliminando código boilerplate e simplificando as funções.

Trigger é um tipo específico de evento que dispara execução da função. Cada função pode ter somente um trigger.

Binding define como a função está conectada com outros serviços da Azure. Não são obrigatórios. Uma função pode ter múltiplos bindings de entrada e saída.

<pre class="mermaid">
  graph TB
    sq[Input Bindings]

    subgraph Input Bindings
        ci((http)) --> Input1
        'ci(Blob Storage) --> Input2
    end

    Input1 --> sq[Trigger]
    Input2 --> sq[Trigger]

    subgraph Function App
        ''ci(function1)
        '''ci(function2)
        ''''ci(function3)
        '''''ci(function4)
    end

    sq[output]
    sq[Trigger] --> ''ci(function1)
    ''ci(function1) --> Output

    Output --> ''''''ci((http)) 

    subgraph Output Bindings
        ''''''ci((http)) 
    end
    </pre>


Todos os triggers e bindings são definidos no arquivo de configuração function.json, onde é obrigatório informar a direction do componente. Direction do trigger é sempre "in". Direction dos bindings pode ser in, out ou inout.
Os trigger são definidos com o sufixo "Trigger" no campo type. Exemplo: "type": "xxxTrigger". (Que gambi!)

Existe uma Integrate Tab no portal para visualizar os triggers e bindings das funções.

Também é possível definir Binding Expressions no functions.json

onde %texto% é o padrão para se referir a variáveis de ambiente e
{texto} é o padrão para demais formatos.

Exemplos:
%input_queue_name%
sample/{filename}
sample/{queueTrigger}
strings/{Blobname}  <-- { "Blobname": "HW.txt" }
input/{rand-guid}  <-- novo UUID
input/{DateTime}  <-- System date Time

LOCAL SETTINGS FILE

Define as configurações da função para execução em ambiente local.
Arquivo local.settings.json deve ficar armazenado no diretório root do projeto. Esse arquivo não deve nunca ser armazenada em um repositório remoto porque pode conter secrets.


{
  "IsEncrypted": false, // quando true, todos os valores são criptografados com uma chave local
  "Values": { "k": "v", k": "v" ... } // Configurações da aplicação
  "Host": { "k": "v", k": "v" ... } // Configurações para execução local da função
  "ConnectionStrings": usado por frameworks compatíveis
}

PRINCIPAIS FERRAMENTAS DO AZURE FUNCTION:

Ferramentas disponíveis para interação via CLI:

func init, func logs, func deploy, func new, func run... etc.

Existem grupos de comandos, cada um possui seu próprio subgrupo:
func azure, func durable, func extensions, func kubernetes, func settings, func templates

CUSTOM HANDLERS

No arquivo function.json é possível definir uma aplicação customizada para executar a função, como um webserver. O pré requisito é que a aplicação esteja preparada para lidar com requisições http.

No arquivo function.json: $.customHandler.description.defaultExecutablePath

::: mermaid
flowchart TB
  subgraph Input
    ci((http))-->|1-Trigger|Function
    end
  subgraph Output
    Function-->|4- Output|ci1((http))
    end
  subgraph Custom Handler 
    Function-->|2- Http request|Executor
    Executor-->|3- Http response|Function
    end
:::

Esse tipo de configuração é super complexo de se fazer. Em geral não vale a pena.
