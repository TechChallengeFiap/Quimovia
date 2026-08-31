# Quimovia

Projeto acadêmico desenvolvido para o Tech Challenge da pós-graduação em Desenvolvimento Full Stack da FIAP.

## Sobre o projeto

A **entrada de cargas químicas** em um porto **envolve diferentes etapas**, como cadastro, envio e análise de documentos, inspeção, avaliação técnica e autorização para movimentação. **Quando essas informações são controladas manualmente** ou permanecem dispersas, **torna-se mais difícil identificar pendências, responsabilidades e o histórico de cada operação**.

O **Quimovia** é uma proposta de plataforma para **centralizar e organizar esse processo** no Porto de Santos. A solução permitirá **gerenciar produtos e cargas químicas, documentação, inspeções, pareceres técnicos, usuários e permissões**, além de manter a rastreabilidade das ações realizadas durante o fluxo operacional.

Uma **carga química pode reunir um ou mais itens**, associados ao **mesmo produto ou a produtos diferentes**, cada um com sua **quantidade e unidade de medida**. A documentação e as inspeções podem abranger **a carga inteira ou itens específicos**, conforme os requisitos definidos.

Nesta primeira fase, o projeto está concentrado na **compreensão e modelagem do domínio**. O repositório reúne as regras de negócio, os casos de uso, os diagramas, a arquitetura proposta e o planejamento de qualidade que orientarão a implementação da aplicação nas próximas etapas.

## Fluxo principal

De forma resumida, a solução foi pensada para apoiar as seguintes etapas:

1. **cadastro dos produtos químicos** e de seus requisitos;
2. **registro da carga** e dos itens que a compõem;
3. **envio e análise da documentação** exigida para a carga e seus itens;
4. **realização das inspeções** aplicáveis;
5. **avaliação das evidências e emissão do parecer técnico** pelo Responsável Técnico atribuído à carga;
6. **autorização operacional da movimentação**, após aprovação técnica e atendimento aos requisitos.

A **aprovação técnica e a liberação operacional são decisões distintas**: um parecer favorável não autoriza automaticamente a movimentação da carga.

Cada etapa será executada por usuários autorizados, de acordo com as responsabilidades e permissões definidas no domínio. O sistema aplica validações e registra as decisões dos profissionais, **sem substituir a avaliação humana**. O **acompanhamento do status e do histórico** ocorre durante todo o fluxo.

## Objetivos

- Centralizar as informações relacionadas às cargas químicas.
- Organizar o fluxo de documentação, inspeção e avaliação técnica.
- Aplicar regras de negócio e permissões de acesso de forma consistente.
- Registrar alterações e decisões para garantir rastreabilidade.
- Preparar uma base arquitetural que facilite testes, manutenção e evolução.

## Escopo da Fase 1

Esta fase contempla a **documentação e o planejamento da solução**:

- entendimento do problema e do domínio;
- modelagem com Domain-Driven Design (DDD);
- definição de atores, casos de uso e regras de negócio;
- elaboração dos diagramas do sistema;
- definição da arquitetura e da organização inicial do projeto;
- planejamento da estratégia de qualidade e testes;
- definição de como JavaScript avançado e TypeScript serão aplicados na implementação;
- registro das principais decisões arquiteturais.

## Documentação

| Seção | Documento | Conteúdo |
|---:|---|---|
| 01 | [Entendimento do domínio](docs/01-entendimento-do-dominio.md) | Problema, objetivo, atores e processos. |
| 02 | [Modelagem com DDD](docs/02-modelagem-com-ddd.md) | Contextos, entidades, agregados e objetos de valor. |
| 03 | [Regras de negócio](docs/03-regras-de-negócio.md) | Restrições e comportamentos que orientam o sistema. |
| 04 | [Casos de uso](docs/04-casos-de-uso.md) | Ações de atores, entradas, saídas, regras e exceções. |
| 05 | [Diagramas](docs/05-diagramas.md) | Visão consolidada dos modelos e fluxos apresentados anteriormente. |
| 06 | [Arquitetura proposta](docs/06-arquitetura-proposta.md) | Estrutura técnica escolhida para implementar o domínio. |
| 07 | [Organização do projeto](docs/07-organizacao-do-projeto.md) | Como a arquitetura será refletida nas pastas e módulos. |
| 08 | [Decisões arquiteturais](docs/08-decisoes-arquiteturais.md) | Justificativas para DDD, Clean Architecture, monólito modular e tecnologias. |
| 09 | [Aplicação de JavaScript e TypeScript](docs/09-aplicacao-javascript-avancado-e-typescript.md) | Como os recursos da linguagem serão usados na implementação. |
| 10 | [Planejamento de qualidade](docs/10-planejamento-de-qualidade-de-software.md) | Estratégias de testes, cenários críticos e critérios de qualidade. |

## Arquitetura e tecnologias planejadas

O Quimovia será iniciado como um **monólito modular**, orientado por **DDD** e **Clean Architecture**. A separação entre domínio, aplicação, apresentação e infraestrutura busca **preservar as regras de negócio** e **facilitar a evolução da solução** sem introduzir complexidade desnecessária nesta etapa.

| Área | Tecnologias |
|---|---|
| Frontend | Vue.js, TypeScript, Vuetify, Pinia, Vue Router, Sass (SCSS) e Vite |
| Backend | NestJS, TypeScript e Prisma |
| Testes | Jest |
| Infraestrutura | Docker, GitHub Actions e Google Cloud Platform |
| Documentação | Markdown, Mermaid, Draw.io e Swagger |
| Banco de dados | A definir |

## Equipe

### Grupo 7

- Letícia Gabrielle Videira dos Santos - (RM: 376263)
- Matheus Felicio Brazolin - (RM: 375006)
- Paulo Henrique dos Santos Milano - (RM: 375284)
- Ygor Takashi Nishi - (RM: 375939)
