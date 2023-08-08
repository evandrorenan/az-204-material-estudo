AZURE FUNCTIONS - UNREACHABLE

Esse é um problema que ocorre quando a função não consegue inicializar a sua Runtime (jvm, por exemplo).

Causas comuns:
- Algum problema com a storage account - foi deletada, as configurações foram deletadas, as credenciais estão inválidas ou está inacessível por algum outro motivo
- Estourou a cota de execução diária
- Firewall

DURABLE FUNCTIONS

Permite a criação de funções serverless statefull

São divididas em dois tipos

Orchestatior function - define workflows que mantém estado
Entity function - controlam o estado de uma entidade.

Os workflows são definidos no código, não possuem JSON de configuração (Azure define esse ponto como uma vantagem, mas outros provedores possuem soluções no-code baseadas em JSON)

Outras funções podem ser chamadas tanto de forma síncrona quanto assíncrona.
Elas automaticamente salvam (checkpoint) o ponto de progresso sempre que uma function para (await). O estado local não é perdido mesmo se o processo for reclicado ou a VM reiniciada.

Compatíveis com as linguagens abaixo (não tem Java =/)

C#, Javascript, Python, F#, Powershell, Powershell 7

Uma lib específica é necessária na aplicação para utilização de durable-functions 

PADRÕES DE IMPLEMENTAÇÃO DE DURABLE FUNCTIONS

Function chaining - Várias funções são executadas, uma após a outra, podendo a saída de uma ser a entrada da próxima.

::: mermaid
flowchart LR
  A(input)-->B[func1]
  B[func1]-->C(output/input)
  C(output/input)-->D[func2]
  D[func2]-->E(output/input)
  E(output/input)-->F[func3]
  F[func3]-->G(output)
:::

Fan-out/fan-in - múltiplas funções são executadas em paralelo e então aguarda-se até todas terminarem.

Fanning out - pode ser implementado com funções normais atualizando uma queue

Fanning in - é mais complexo porque é necessário controlar no código quando uma função é disparada pela fila e armazenar as saídas de cada função. 

::: mermaid
flowchart LR
  A(input)-->B[func1]
  B[func1]-->G(output)
  A(input)-->D[func2]
  D[func2]-->G(output)
  A(input)-->F[func3]
  F[func3]-->G(output)
:::

Azure provê a extensão durable-functions que facilita a atuação com esses padrões

ASYNC HTTP API - Permite consultar o status de processamento de operações com longo tempo de execução (LRO). 

Durable functions provê APIs pre-definidas para simplificar o código nesse tipo de situação.

::: mermaid
flowchart LR
  A(start)-->B[função demorada]-->C(Saída)
  B[função demorada]-->D(Status)
  E(Request Status)-->D(Status)
:::

MONITOR PATTERN

Utilizado em situações em que é necessário definir atividades com execução periódica com condições flexíveis. 

Human interaction

Processos que exigem interação humana (para aprovação, por exemplo), podem utilizar o recurso Automated Process para implementação, definindo timeouts e lógicas de compensação.


Aggregator pattern

Esse padrão permite que você acumule os dados de um evento ocorrido múltiplas vezes em um período de tempo. Os dados são acumulados em uma entidade unificada.
