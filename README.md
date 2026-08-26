# Quimovia

> Projetos acadêmico desenvolvido para o Tech Challenge da pós-graduação em Desenvolvimento Full Stack da FIAP.

## Sobre o projeto

A **entrada de cargas químicas** em um porto **envolve diferentes etapas**, como cadastro, envio e análise de documentos, inspeção, avaliação técnica e acompanhamento da situação da carga. **Quando essas informações são controladas manualmente** ou permanecem dispersas, **torna-se mais difícil identificar pendências, responsabilidades e o histório de cada operação**.

O **Quimovia** é uma proposta de plataforma para **centralizar e organizar esse processo** no Porto de Santos. A solução permitirá **gerenciar produtos e cargas químicas, documentações, inspeções, pareceres técnicos, usuários e permissões**, além de manter a rastreabilidade das ações realizadas durante o fluxo operacional.

Nesta primeira fase, o projeto está concentrado na **compreensão e modelagem do domínio**. O repositório reúne regras de negócio, os casos de uso, os diagramas, a arquitetura proposta e o planejamento de qualidade que orientarão a implementação da aplicação nas próximas etapas.

## Fluxo principal

De forma resumida, a solução foi pensada para apoiar as seguintes etapas:

1. regristro da carga e dos produtos químicos associados;
2. envio e análise da documentação obrigatória;
3. realização das inspeções previstas;
4. emissão dos pareceres necessários;
5. decisão técnica sobre a movimentação da carga;
6. acompanhamento do status e do histórico da operação.

Cada etapa será executada por usuários autorizados, de acordo com as responsabilidades e permissões definidas no domínio.

## Objetivos

- Centralizar as informações relacionadas às cargas químicas.
- Organizar o fluxo de documentação, inspeção e avaliação técnica.
- Aplicar regras de negócio e permissões de acesso de forma consistente.
- Registrar alterações e decisões para garantir rastreabilidade.
- Preparar uma base arquitetural que facilite testes, manutenção e evolução.

## Escopo da Fase 1

Esta fase contempla a documentação e o planejamento da solução

- entendimento do problema e do domínio;
- modelagem orientada à Domain-Driven Design (DDD);
- definição de atores, casos de uso e regras de negócio;
- elaboração dos diagramas do sistema;
- definição da arquitetura e da organização inicial do projetos;
- planejamento da estratégia de qualidade e testes;
- definição da como JavaScript avançado e TypeScript serão aplicados na implementação;
- registro das principais decisões arquiteturais.

## Documentação

| Seção | Documento | Conteúdo |
|---:|---|---|
| 01 | [Entendimento do domínio](docs/01-entendimento-do-dominio.md) | Problema, objetivo, atores, processos e linguagem utilizada. |
| 02 | Modelagem com DDD | Contextos, entidades, agregados e objetos de valor. |
| 03 | Regras de negócio | Restrições e comportamentos que orientam o sistema. |
| 04 | Casos de uso | Ações de atores, entradas, saídas, regras e exceções. |
| 05 | Diagramas | Visão consolidada dos modelos e fluxos apresentados anteriormente. |
| 06 | Arquitetura proposta | Estrutura técnica escolhida para implementar o domínio. |
| 07 | Organização do projeto | Como a arquitetura será refletida nas pastas e módulos. |
| 08 | Decisões arquiteturais | Justificativas para DDD, Clean Architecture, monólito modular e tecnologias. |
| 09 | Aplicação de JavaScript e TypeScript | Como os recursos da linguagem serão usados na implementação. |
| 10 | Planejamento de qualidade | Estratégias de testes, cenários críticos e critérios de qualidade. |

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
