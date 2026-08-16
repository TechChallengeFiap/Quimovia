# Organização do Projeto

O projeto será organizado de forma modular, separando a documentação técnica do código-fonte e da suíte de testes.

Este mesmo repositório será utilizado para centralizar a documentação, os diagramas e, futuramente, a implementação da aplicação.

## Estrutura inicial

```text
docs/
src/
├── domain/
│   ├── entities/
│   └── value-objects/
│  
│
├── application/
│   ├── use-cases/
│   └── contracts/
│
├── infrastructure/
|   └── repositories/
|
├── presentation/
│   ├── dtos/
|   └── controllers/
│
└── shared/
    └── types/
tests/
└── mocks/
README.md
```

## docs

O diretório docs concentrará toda a documentação do projeto, incluindo descrições de domínio, casos de uso, regras de negócio, diagramas e decisões arquiteturais.

## src

O diretório src armazenará o código-fonte da aplicação, organizado em camadas com base na Clean Architecture.

### domain

Cada módulo de domínio será separado em subdiretórios próprios, com o objetivo de concentrar entidades, objetos de valor, agregados e regras de negócio, sem dependências externas.

#### entities
Contém as entidades do domínio, representando os conceitos centrais da aplicação.

#### value-objects
Contém os objetos de valor do domínio, representando conceitos que não possuem identidade própria, mas que são importantes para a modelagem do negócio.

### application

Cada módulo do domínio terá um correspondente na camada de aplicação, responsável por orquestrar os casos de uso e a comunicação com portas e serviços externos.

#### use-cases
Contém os casos de uso da aplicação, implementando a lógica de negócio e coordenando as interações entre entidades, objetos de valor e repositórios.

#### contracts
Contém os contratos e interfaces utilizados pelos casos de uso, definindo as dependências externas da aplicação.

### infrastructure

Cada módulo do domínio terá um correspondente na camada de infraestrutura, responsável por fornecer implementações concretas de repositórios, integrações e demais dependências externas.

#### repositories
Contém as interfaces de repositórios do domínio, definindo contratos para persistência e recuperação de entidades e agregados, sem depender de implementações concretas.

### presentation

Cada módulo do domínio terá um correspondente na camada de apresentação, responsável por expor APIs, rotas, controladores e futuras interfaces de entrada da aplicação.

#### dto
Contém os Data Transfer Objects (DTOs) utilizados para comunicação entre a aplicação e serviços externos, como APIs, bancos de dados e filas de mensagens.

#### controllers
Contém os controladores responsáveis por receber requisições, validar dados de entrada e coordenar a execução dos casos de uso, retornando respostas apropriadas para os clientes.

### shared

Contém componentes compartilhados entre diferentes módulos, como utilitários, constantes, contratos e classes base.

#### types
Contém tipos compartilhados entre diferentes módulos, como interfaces, tipos genéricos e classes base.

## tests

O diretório tests reunirá os testes unitários e de integração do projeto, seguindo a mesma organização modular adotada no código-fonte.

### mocks
Contém implementações simuladas de repositórios, serviços externos e quaisquer outras dependências que não sejam o foco do teste, permitindo isolar o comportamento das unidades testadas.


