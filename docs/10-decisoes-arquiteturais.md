# Decisões arquiteturais
Neste documento apresenta as decisões arquiteturais tomadas durante o desenvolvimento do projeto com os seguintes tópicos:

## Por que separar domínio, aplicação e infraestrutura?
Separar essas 3 camadas segue o princípio da responsabilidade única (SRP) do Domain Driven Design (DDD), onde o domínio é responsável por representar o negócio como as entidades, agregados e suas funcionalidades, a aplicação é responsável por orquestrar as regras de negócio conversando com o cliente e redirecionando para as outras camadas, e a infraestrutura é responsável por fornecer os recursos necessários para que a aplicação funcione, como banco de dados, APIs, serviços externos, e etc.

## Por que concentrar regras de negócio no domínio ou nos casos de uso?
Optar por concentrar regras de negócio tanto no domínio quanto nos casos de uso, depende muito do contexto do projeto. Geralmente concentrar regras no domínio torna eles mais independentes e desacoplados, enquanto concentrar as regras de negócio nos casos de uso **CONTINUAR AQUI.**

## Por que utilizar TypeScript?
O nos trará as vantagens do JavaScript, e aumentando a segurança com a tipagem forte, nos auxiliando em criações de classes, interfaces, tipos e parâmetros. Temos a vantagens de criar objetos de valores com os Enums e também criar tipos Generics.

No TypeScript fica facilitado o controle de funções assíncronas e as funções puras, além de ter várias bibliotecas que nos auxilia na criação de testes.

O desempenho do TypeScript é outro fator importante, pois **CONTINUAR AQUI.**

## Como o projeto poderá evoluir para backend?
O projeto irá evoluir para o backend utilizando uma stack ja discutida e definida utilizando NestJS, Prisma, Jest e TypeScript. A ideia é começar com um monolito, ou seja, iremos fazer um projeto com uma arquitetura de fácil desacoplamento pensando em evoluir para microserviços. Nesse projeto será utilizado o padrão Clean Architecture, criando uma API RESTful, para os usuários controlar as ações sob a carga, o usuário externo cadastrar uma nova carga e termos um gerenciamento de usuários.

## Como o projeto poderá evoluir para frontend?
O projeto irá evoluir para o frontend utilizando uma stack ja discutida e definida utilizando VueJS, Vuetify e TypeScript. A ideia é criar um SPA (Single Page Application). **CONTINUAR AQUI.**

## Como o projeto poderá evoluir para mobile?
**Continuar AQUI.**

## Como o projeto poderá evoluir para microsserviços?
**Continuar AQUI.**

## Como o grupo pretende evitar acoplamento excessivo?
O grupo irá seguir o princípio da responsabilidade única (SRP) do Domain Driven Design (DDD), o projeto seguirá a arquitetura Clean Architecture, onde cada camada terá sua responsabilidade e não irá depender de outras camadas. Além disso, a stack que nós iremos utilizar: NestJS junto ao TypeScript, nos ajudará com a injeção de dependência (Dependency Injection - DI).  


