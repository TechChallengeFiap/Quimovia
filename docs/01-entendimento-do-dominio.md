# Entendimento do domínio

## Contexto

A **entrada e a movimentação de cargas químicas** em um porto exigem o controle de produtos, responsáveis, documentos, inspeções e decisões. Essas informações **precisam permanecer relacionadas durante todo o processo** para que os envolvidos consigam identificar a situação de cada carga, suas pendências e as ações já realizadas.

O **Quimovia** é uma proposta acadêmica de sistema para apoiar esse fluxo no Porto de Santos. A solução busca **reunir as informações operacionais em um único ambiente e manter o histórico de cada carga**, desde o registro até as decisões técnica e operacional sobre sua movimentação.

Uma carga é composta por **um ou mais itens**, que podem referenciar o mesmo produto químico ou produtos diferentes. Cada item possui sua própria **quantidade e unidade de medida**. Documentos e inspeções podem ser exigidos para a **carga inteira ou para itens específicos**, conforme os requisitos aplicáveis.

## Problema

Processos manuais e informações distribuídas entre diferentes registros **dificultam o acompanhamento das cargas químicas**. Esse cenário pode causar:

- perda ou duplicidade de informações;
- dificuldade para identificar documentos, inspeções e itens com pendências;
- falta de clareza sobre as responsabilidades de cada participante;
- alterações de status sem critérios consistentes;
- dificuldade para recuperar as evidências que fundamentaram uma decisão.

Quando uma carga contém vários itens, conhecer apenas sua situação geral não é suficiente: é necessário identificar **qual requisito está pendente e a que parte da carga ele se aplica**.

## Objetivo

O Quimovia tem como objetivo **centralizar o cadastro e o acompanhamento das cargas químicas e de seus itens**, organizando as etapas de documentação, inspeção, avaliação técnica e liberação operacional. O sistema deverá aplicar as regras do fluxo, controlar o acesso às funcionalidades e registrar as ações relevantes para garantir a **rastreabilidade**, sem substituir a avaliação dos profissionais responsáveis.

## Usuários envolvidos

| Usuário | Responsabilidade principal |
|---|---|
| **Embarcador** | Registrar a carga com seus itens e enviar a documentação solicitada. |
| **Analista** | Avaliar a documentação e emitir pareceres documentais para os escopos exigidos. |
| **Fiscal** | Realizar as inspeções sob sua responsabilidade e emitir os laudos correspondentes. |
| **Responsável Técnico** | Avaliar o conjunto de evidências e emitir o parecer técnico da carga à qual está atribuído. |
| **Operador Portuário** | Acompanhar o fluxo operacional e a situação das cargas. |
| **Gestor Operacional** | Supervisionar as operações e consultar informações gerenciais e de auditoria. |
| **Administrador do Sistema** | Gerenciar usuários, perfis e permissões de acesso. |

A gestão do catálogo, a atribuição de responsáveis e a liberação operacional exigem **permissões específicas**, cujos perfis responsáveis ainda dependem de validação do grupo. As permissões e demais atribuições serão detalhadas nas [Regras de negócio](./03-regras-de-negócio.md) e nos [Casos de uso](./04-casos-de-uso.md), sem presumir que o acesso à consulta autorize alterações ou decisões.

## Fluxo principal

O processo começa pela definição dos produtos e dos requisitos aplicáveis e avança conforme as evidências são analisadas:

1. **Preparação do catálogo:** os produtos químicos, suas características e os requisitos documentais e de inspeção são cadastrados.
2. **Registro da carga:** o Embarcador informa os itens, os produtos associados e as quantidades com suas unidades de medida.
3. **Análise documental:** o Embarcador envia a documentação exigida; o Analista avalia os escopos aplicáveis e registra os pareceres documentais.
4. **Inspeção:** após a conformidade documental, os Fiscais responsáveis realizam as verificações exigidas para a carga e seus itens e emitem os laudos.
5. **Avaliação técnica:** com as evidências obrigatórias concluídas e sem pendências impeditivas, o Responsável Técnico atribuído avalia o conjunto e emite o parecer favorável ou desfavorável.
6. **Liberação operacional:** se a carga estiver aprovada tecnicamente e sem impedimentos, um usuário autorizado registra a permissão para movimentação.

**Aprovação técnica não equivale à liberação operacional.** A primeira registra a conclusão técnica favorável; a segunda autoriza a movimentação. Essa autorização também não comprova que a movimentação física já ocorreu.

Pendências exigem tratamento antes do avanço correspondente. Uma pendência em um único item pode impedir o avanço da carga inteira, e a conformidade de outro item não a resolve, mesmo que ambos referenciem o mesmo produto. O **status e o histórico permanecem disponíveis durante todo o processo**, inclusive nas correções e reavaliações.

## Escopo e informações controladas

| Área | Responsabilidade do sistema |
|---|---|
| **Produtos Químicos** | Manter identificação, classificação de risco, estado físico, situação cadastral e requisitos associados. |
| **Cargas Químicas** | Registrar itens, produtos referenciados, quantidades e unidades, embarcador, responsável técnico, datas, pendências e status. |
| **Documentação** | Controlar documentos obrigatórios, arquivos enviados, escopos atendidos e pareceres documentais. |
| **Inspeções** | Registrar inspeções exigidas, escopos avaliados, fiscais responsáveis, resultados e laudos. |
| **Avaliação Técnica** | Registrar o parecer técnico da carga, sua autoria e as evidências consideradas na decisão. |
| **Liberação Operacional** | Registrar a autorização de movimentação, separadamente da aprovação técnica. |
| **Acesso** | Gerenciar usuários, perfis e permissões. |
| **Auditoria** | Manter o histórico das ações, alterações e decisões relacionadas à carga e aos registros envolvidos. |

Na **Fase 1**, o projeto está concentrado na compreensão do domínio e no planejamento da solução. A implementação da aplicação será realizada **nas etapas posteriores do projeto**.

## Decisões, riscos e restrições

O sistema deverá organizar as informações necessárias à análise, identificar requisitos não atendidos e validar as condições de avanço. **Pareceres, laudos e autorizações continuam sob responsabilidade dos profissionais habilitados para cada ação.**

O fluxo se apoia em quatro cuidados principais:

- **Acesso e proteção:** cada operação exige usuário autenticado, ativo e autorizado, com proteção das informações contra acesso e alteração indevidos.
- **Avaliação do conjunto:** os requisitos da carga e de seus itens devem ser considerados em conjunto; um resultado individual não comprova a conformidade dos demais escopos.
- **Avanço controlado:** as mudanças de status dependem das condições definidas para cada etapa. Uma pendência ainda não atendida não equivale, por si só, ao registro de um bloqueio formal.
- **Preservação do histórico:** alterações relevantes devem ser rastreáveis. Correções e reavaliações não podem apagar as evidências e decisões anteriores nem estender automaticamente seus resultados a uma composição modificada.

As condições detalhadas de alteração, bloqueio, retomada e liberação pertencem ao tópico de **Regras de negócio**. Requisitos de natureza regulatória adotados na proposta têm finalidade acadêmica e deverão ser validados por especialistas antes de uma eventual utilização em ambiente real.

## Evolução prevista

Nas próximas fases, o Quimovia poderá evoluir para uma **aplicação web responsiva**, com área administrativa, armazenamento de documentos, notificações e integrações externas. Também poderá ser desenvolvida uma **aplicação móvel** para consultas e atividades realizadas no local da operação.

A evolução para microsserviços deverá ser avaliada somente se o crescimento e as necessidades da solução justificarem essa complexidade. As escolhas técnicas serão tratadas nos documentos de arquitetura, sem alterar as responsabilidades do negócio descritas aqui.
