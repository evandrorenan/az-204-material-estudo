::: mermaid
graph LR
    A[Início] --> B[Processo]
    B --> C[Decisão]
    C --> D[Final]
:::

::: mermaid
graph TD
    A --> B
    B -->|Opção 1| C
    B -->|Opção 2| D
    C --> D
:::

::: mermaid
graph TD
    A --> B
    B -->|Opção 1| C
    B -->|Opção 2| D
    C --> D
:::

::: mermaid
graph TD
    subgraph Group A
        A1 --> A2
    end
    subgraph Group B
        B1 --> B2
    end
    A2 --> B1
:::

::: mermaid
sequenceDiagram
    Alice->>Bob: Pedido
    Bob->>John: Pedido recebido
    Bob-->>Alice: Confirmação
:::

::: mermaid
gantt
    title Cronograma de Projeto
    dateFormat  YYYY-MM-DD
    section Fase 1
    Tarefa 1: 2023-08-01, 7d
    Tarefa 2: 2023-08-08, 5d
:::

::: mermaid
classDiagram
    class Pessoa {
        +string nome
        +int idade
        +comer()
        +dormir()
    }
    Pessoa <|-- Estudante
    Pessoa <|-- Professor
:::

::: mermaid
erDiagram
    CUSTOMER ||--o{ ORDER : "places"
    ORDER ||--|{ LINE-ITEM : "contains"
:::

::: mermaid
stateDiagram
    [*] --> State1
    State1 --> [*]
    State1 --> State2
    State2 --> State3 : Event1
    State2 --> State1 : Event2
:::

::: mermaid
graph TD
    A[Começo] --> B{Decisão}
    B -->|Condição 1| C[Atividade 1]
    B -->|Condição 2| D[Atividade 2]
    C --> D[Atividade 2]
    D --> E[Fim]
:::

::: mermaid
graph TD
    style A fill:#F9F,stroke:#333,stroke-width:2px
    A[Caixa com Fundo]
:::

::: mermaid
graph TD
    A[Texto em **Negrito**]
    B[Texto em _Itálico_]
    C[Texto em <u>Sublinhado</u>]
    D[Texto em <font color=red>Vermelho</font>]
:::

::: mermaid
graph TD
    style A fill:#F9F,stroke:#333,stroke-width:2px
    A[Caixa com Fonte Personalizada]:::estiloDaFonte
:::

::: mermaid
graph TD
    A[Caixa com Texto no Topo] -->|Texto| B[Outra Caixa]
    C[Caixa com Texto na Direita] -->|Texto| D[Outra Caixa]
    E[Caixa com Texto na Parte de Baixo] -->|Texto| F[Outra Caixa]
    G[Caixa com Texto na Esquerda] -->|Texto| H[Outra Caixa]
:::

::: mermaid
graph TD
    style A fill:#F9F,stroke:#333,stroke-width:2px,stroke-dasharray: 5
    A[Caixa com Bordas Arredondadas]
:::

