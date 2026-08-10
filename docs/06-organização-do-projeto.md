# Organização do Projeto

O projeto será organizado de forma modular, separando a documentação técnica do código-fonte e da suíte de testes.

Este mesmo repositório será utilizado para centralizar a documentação, os diagramas e, futuramente, a implementação da aplicação.

## Estrutura inicial

```text
docs/
src/
tests/
README.md
```

## Documentação

O diretório docs concentrará toda a documentação do projeto, incluindo descrições de domínio, casos de uso, regras de negócio, diagramas e decisões arquiteturais.

## Código-fonte

O diretório src armazenará o código-fonte da aplicação, organizado em camadas com base na Clean Architecture.

### domain

Cada módulo de domínio será separado em subdiretórios próprios, com o objetivo de concentrar entidades, objetos de valor, agregados e regras de negócio, sem dependências externas.

#### chemical-loads

Contém as entidades e regras de negócio relacionadas às cargas químicas.

### application

Cada módulo do domínio terá um correspondente na camada de aplicação, responsável por orquestrar os casos de uso e a comunicação com portas e serviços externos.

#### chemical-loads

Contém os casos de uso e os serviços de aplicação relacionados às cargas químicas.

### infrastructure

Cada módulo do domínio terá um correspondente na camada de infraestrutura, responsável por fornecer implementações concretas de repositórios, integrações e demais dependências externas.

#### chemical-loads

Contém as implementações de repositórios, serviços externos e demais dependências relacionadas às cargas químicas.

### presentation

Cada módulo do domínio terá um correspondente na camada de apresentação, responsável por expor APIs, rotas, controladores e futuras interfaces de entrada da aplicação.

#### chemical-loads

Contém os controladores, rotas e demais componentes de apresentação relacionados às cargas químicas.

### shared

Contém componentes compartilhados entre diferentes módulos, como utilitários, constantes, contratos e classes base.

## Testes

O diretório tests reunirá os testes unitários e de integração do projeto, seguindo a mesma organização modular adotada no código-fonte.

#### chemical-loads

Contém os testes relacionados às cargas químicas, incluindo cenários unitários e de integração.


