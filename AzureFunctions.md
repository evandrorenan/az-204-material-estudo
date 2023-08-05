Azure Functions é um serviço que  permite aos desenvolvedores focar em escrever código sem se preocupar em manter os componentes de infraestrutura necessarios.

::: mermaid
mindmap
  root(Azure Functions - Tópicos importantes)
    Conceito Serverless
    Serviço com execução baseada em triggers e bindings
    Function Apps
    Armazenamento      
:::

Sobre serverless e Azure Functions:

Serverless é um conceito que pode ser colocado em uma escala.
Um serviço pode ter um determinado grau de aderência ao conceito de serverless.
FaaS, por exemplo, não é automaticamente serverless - somente quando configurada para ser totalmente gerencia e escalável até 0.

::: mermaid
mindmap
  root(Azure Functions - Serverless)
    )O que é Serverless(
      (Mais uma escala. Um serviço pode aderir parcialmente ao conceito de serverless)
        (FaaS não pe automaticamente serverless)
          (FaaS é considerada serverless somente quando configurada para ser totalmente gerencia e escalável até 0)
      (Infraestrutura abstraída)
      (Pode escalar até 0)
      (Permite que desenvolvedores escrevam pequenos trechos de código)
      (Uma aplicação serverless orquestra múltiplas funções serverless)
      (Cobrança com base na execução)
:::

Sobre triggers e bindings

Triggers são eventos definidos para disparar a execução da function.
Bindings - são os datasources que serão passados para a Function quando um trigger for disparado

::: mermaid
mindmap
  root(Azure Functions)
    Trigger e Bindings
      (Triggers)        
        Só é permitido um trigger por Function ))confirmar esse item((
      (Bindings)        
:::

Sobre armazenamento

Toda function requer uma Storage account. Se a service account for excluída a Function não vai mais funcionar

::: mermaid
mindmap
  root(Azure Functions - Armazenamento)
    Blob Storage
      Mantém function keys e binding states
    Azure Files
      Disponível somente em Premium Plans
      Integração via protocolo SMB
    Queue Storage
      Usado em taks hubs de Durable Functions
    Table Storage
      Usado em taks hubs de Durable Functions
:::

Sobre Function Apps

Uma Function App define os recursos de computação de uma coleção de funções, tais como Hosting, Runtime (Versão do java, por exemplo) e outras configurações globais.
Anatomia de uma Function App:

::: mermaid
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

:::

Anatomia de uma função