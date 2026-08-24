# Decisões arquiteturais

Neste documento são apresentadas as decisões arquiteturais tomadas durante o desenvolvimento do projeto com os seguintes tópicos:

## Por que separar domínio, aplicação e infraestrutura?

Separar essas 3 camadas segue o princípio da responsabilidade única (SRP) do Domain Driven Design (DDD), onde o domínio é responsável por representar o negócio como as entidades, agregados e suas funcionalidades, a aplicação é responsável por orquestrar as regras de negócio, conversando com o cliente e redirecionando para as outras camadas, e a infraestrutura é responsável por fornecer os recursos necessários para que a aplicação funcione, como banco de dados, APIs, serviços externos, e etc.

## Por que concentrar regras de negócio no domínio ou nos casos de uso?

Optar por concentrar regras de negócio tanto no domínio quanto nos casos de uso depende muito do contexto do projeto. Geralmente, concentrar regras no domínio torna elas mais independentes e desacopladas, enquanto concentrar as regras de negócio nos casos de uso pode facilitar a organização dos fluxos específicos da aplicação.

Para o QuimiPort, as regras relacionadas ao comportamento e às restrições do negócio serão concentradas principalmente no domínio, através das entidades, agregados e objetos de valor. Já os casos de uso serão responsáveis por orquestrar essas regras, coordenando as ações necessárias para realizar uma determinada operação.

Por exemplo, a regra que determina se uma carga pode ser aprovada pertence ao domínio da carga química, enquanto o caso de uso de aprovação será responsável por buscar a carga, executar a operação de aprovação e persistir o resultado.

Dessa forma, o domínio permanece independente das demais camadas, enquanto os casos de uso ficam responsáveis por coordenar o fluxo das operações.

## Por que utilizar TypeScript?

O TypeScript nos trará as vantagens do JavaScript, aumentando a segurança com a tipagem forte e nos auxiliando na criação de classes, interfaces, tipos e parâmetros. Temos também a vantagem de criar objetos de valor com os Enums e utilizar tipos Generics quando necessário.

No TypeScript fica facilitado o controle de funções assíncronas e funções puras, além de existirem várias bibliotecas que nos auxiliam na criação e execução de testes.

A manutenção do código também é facilitada pela tipagem estática, pois muitos erros podem ser identificados durante o desenvolvimento antes mesmo da execução da aplicação. Além disso, o TypeScript possui uma boa integração com a stack escolhida pelo grupo, como NestJS, Prisma e Jest.

O desempenho do TypeScript não será considerado uma vantagem direta, pois o TypeScript é convertido para JavaScript antes da execução. Dessa forma, a principal vantagem para o projeto está na segurança de tipos, produtividade e facilidade de manutenção do código.

## Como o projeto poderá evoluir para backend?

O projeto irá evoluir para o backend utilizando uma stack já discutida e definida utilizando NestJS, Prisma, Jest e TypeScript. A ideia é começar com um monólito, ou seja, iremos fazer um projeto com uma arquitetura de fácil desacoplamento pensando em evoluir para microserviços.

Nesse projeto será utilizado o padrão Clean Architecture, criando uma API RESTful para os usuários controlarem as ações sobre a carga, o usuário externo cadastrar uma nova carga e termos um gerenciamento de usuários.

A separação entre as camadas permitirá que as regras de negócio permaneçam independentes de detalhes de implementação, facilitando futuras alterações na infraestrutura e na tecnologia utilizada.

## Como o projeto poderá evoluir para frontend?

O projeto irá evoluir para o frontend utilizando uma stack já discutida e definida utilizando VueJS, Vuetify e TypeScript. A ideia é criar uma SPA (Single Page Application).

O frontend será responsável pela interface com os usuários, permitindo realizar operações como cadastro de produtos, registro e acompanhamento de cargas, consulta de documentações e acompanhamento dos status das cargas.

A aplicação frontend irá se comunicar com o backend por meio da API RESTful, evitando que as regras de negócio fiquem diretamente implementadas na interface.

Dessa forma, alterações na interface não precisarão alterar diretamente as regras de negócio ou a estrutura interna do domínio.

## Como o projeto poderá evoluir para mobile?

A arquitetura será planejada para permitir futuramente a criação de uma aplicação mobile consumindo a mesma API RESTful utilizada pelo frontend web.

Dessa forma, o aplicativo mobile poderá utilizar os mesmos casos de uso e regras de negócio disponibilizados pelo backend, evitando a duplicação das regras entre diferentes aplicações.

A tecnologia utilizada para o desenvolvimento mobile poderá ser definida futuramente de acordo com as necessidades do projeto.

## Como o projeto poderá evoluir para microsserviços?

O projeto será inicialmente desenvolvido como um monólito modular, mantendo uma separação clara entre os domínios identificados durante a modelagem.

Atualmente, o sistema foi dividido nos seguintes domínios:

* Produtos Químicos;
* Cargas Químicas;
* Documentação das Cargas e Produtos;
* Usuários e Operações.

Essa separação facilitará uma possível evolução para microsserviços no futuro, caso o crescimento da aplicação justifique essa arquitetura.

Cada domínio poderá futuramente ser separado em um serviço independente, permitindo maior autonomia, escalabilidade e possibilidade de deploy individual.

Entretanto, a utilização de microsserviços não será feita inicialmente, pois adicionaria uma complexidade desnecessária para o estágio atual do projeto. A decisão de realizar essa migração deverá considerar fatores como crescimento da aplicação, necessidade de escalabilidade e independência dos domínios.

## Como o grupo pretende evitar acoplamento excessivo?

O grupo irá seguir o princípio da responsabilidade única (SRP) do Domain Driven Design (DDD), o projeto seguirá a arquitetura Clean Architecture, onde cada camada terá sua responsabilidade bem definida.

Além disso, a stack que nós iremos utilizar, NestJS junto ao TypeScript, nos ajudará com a injeção de dependência (Dependency Injection - DI).

Também serão utilizadas interfaces para definir contratos entre componentes, permitindo que uma implementação possa ser substituída sem alterar as regras de negócio.

A separação entre domínio, aplicação, apresentação e infraestrutura também ajudará a evitar que as regras de negócio dependam diretamente de banco de dados, APIs externas ou frameworks.

Dessa forma, o projeto poderá evoluir suas implementações sem exigir grandes alterações nas demais camadas, mantendo o sistema mais flexível e facilitando a criação de testes.
