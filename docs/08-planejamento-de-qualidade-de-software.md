# Planejamento de qualidade de software
Este documento apresenta o planejamento de qualidade de software do QuimiPort, definindo como a aplicação será validada nas próximas fases do projeto.

O objetivo é garantir que as regras de negócio sejam respeitadas, que os principais casos de uso funcionem corretamente e que os fluxos críticos da aplicação sejam testados de forma automatizada.

## Regras de Negócio que Devem ser Testadas

As principais regras de negócio identificadas no domínio deverão possuir cenários de teste correspondentes.

| Código | Regra de Negócio                                                     | Prioridade |
| ------ | -------------------------------------------------------------------- | ---------- |
| RN-001 | Uma carga química deve possuir um produto químico associado.         | Alta       |
| RN-002 | Um produto químico inativo não pode ser utilizado em novas cargas.   | Alta       |
| RN-003 | Uma carga química deve possuir classificação de risco.               | Média      |
| RN-004 | Uma carga não pode ser liberada sem a documentação obrigatória.      | Alta       |
| RN-005 | Uma carga bloqueada não pode entrar em movimentação.                 | Alta       |
| RN-006 | Uma carga reprovada não pode ser liberada.                           | Alta       |
| RN-007 | A quantidade de produtos na carga deve ser maior que zero.           | Média      |
| RN-008 | Toda carga deve possuir um responsável técnico.                      | Alta       |
| RN-009 | Um produto químico deve possuir nome e classe de risco.              | Alta       |
| RN-010 | O status da carga deve seguir as transições permitidas pelo domínio. | Alta       |
| RN-011 | Uma carga não pode ser liberada sem as inspeções obrigatórias.       | Alta       |
| RN-012 | Apenas cargas que atendam às regras de negócio possam ser aprovadas ou liberadas. | Alta       |
---
As regras deverão ser testadas tanto para cenários válidos quanto para cenários inválidos.

## Casos de uso críticos
Os casos considerados mais críticos são:
- Registro de carga química;
- Associar produto químico à carga química;
- Validar documentação da carga química;
- Validar inspeção da carga química;
- Alterar o status da carga química;

## Tipos de teste
Iremos utilizar os 3 tipos de teste: unitário, integração e end-to-end. Garantindo os diferentes níveis de teste e a cobertura de todas as regras de negócio.

##  Estratégia para testes unitários
Os testes unitários deverão priorizar as regras de negócio do domínio.

Cada regra deverá possuir cenários que validem tanto o comportamento esperado quanto as situações que devem ser rejeitadas.

Exemplo:

aprovarCarga()

Cenário válido:
- Documentação obrigatória válida
- Inspeção aprovada
- Responsável técnico definido

Resultado esperado:
- Carga aprovada

Cenário inválido:
- Documentação obrigatória ausente

Resultado esperado:
- Operação rejeitada

As dependências externas deverão ser isoladas utilizando mocks ou implementações simuladas.

## Estratégia para testes de integração futuramente
Os testes de integração serão aplicados futuramente para garantir que os componentes funcionem corretamente quando utilizados em conjunto.

Exemplos:

- Registrar uma carga e associar os produtos químicos a ela, validando que a carga foi registrada corretamente e que os produtos foram associados.

Eles deverão testar todos os casos de uso que envolvam múltiplos componentes garantindo a regra de negócio, deverá ser pensado uma forma de unir os testes unitários em um teste de integração, para que seja possível validar o comportamento esperado.

## Validação dos Fluxos Principais
Os principais fluxos do domínio deverão possuir cenários automatizados. para isso iremos utilizar os testes end-to-end, garantindo que o comportamento esperado seja validado.

## Mocks e Dados Simulados
Os Mocks e dados simulados estarão em uma pasta separada dentro de tests, nele estarão dados ficticios que serão utilizados para isolar dependências externas durante os testes.

Nós iremos simular repositórios, serviços externos e qualquer outra dependência que não seja o foco do teste.

Exemplos de dados simulados:

Produto Químico:
- Nome: Produto de teste
- Classe de risco: Classe 3
- Status: Ativo

Carga:
- Produto: Produto de teste
- Quantidade: 100
- Status: Em análise
- Responsável técnico: Usuário de teste

Os dados deverão ser independentes dos dados reais utilizados em ambientes de desenvolvimento ou produção.

## Critérios de Qualidade

Durante a evolução do projeto, serão considerados como critérios de qualidade:

- % de cobertura de testes unitários e de integração;
- Casos de uso críticos testados;
- Fluxos principais validados;
- Tratamento de erros e exceções implementado;
- Execução de testes automatizados em pipelines de integração contínua;