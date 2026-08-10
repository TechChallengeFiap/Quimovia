# Diagramas
Para facilitar a compreensão da solução proposta, primeiro iremos apresentar os dois diagramas necessários do tech challenge e em sequência os diagramas de casos de uso que fizemos para cada dominio para explicar os comportamentos de cada um.

## Fluxo de transição de status da carga:
```mermaid
---
config:
  theme: redux
---
flowchart TB
    A(["Start"]) -- Embarcador: Registra Carga e Documentações --> B["Em_Analise"]
    B --> C{"Valida Documentaçao"}
    C --> D["Em_Inspecao"]
    C --> E["Reprovada"]
    C --> F["Bloqueada"]
    F --Atualiza documentação--> B
    D --> G{"Valida Segurança e Qualidade"}
    H --> E
    G --> E
    G --> H["Liberada"]
    H --Aprovação concedida--> I["Aprovada"]
    I --> J(["End"])
    E --> J
```