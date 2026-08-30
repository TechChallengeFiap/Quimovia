# Entendimento do domínio

## Contexto

A entrada e a movimentação de cargas químicas em um porto exigem o **controle** de produtos, responsáveis, documentos, inspeções e decisões técnicas. Essas informações **precisam permanecer relacionadas durante todo o processo** para que os envolvidos consigam **identificar a situação da carga**, suas pendências e as ações já realizadas.

O **Quimovia** é uma proposta acadêmica de sistema para apoiar a gestão desse fluxo no Porto de Santos. A solução busca **reunir as informações operacionais em um único ambiente e manter o histórico de cada carga** desde o registro até a decisão sobre sua movimentação.

## Problema

Processos manuais e informações distribuídas entre diferentes registros **dificultam o acompanhamento das cargas químicas**. Esse cenário pode causar:

- perda ou duplicidade de informações;
- dificuldade para identificar documentos e inspeções pendentes;
- falta de clareza sobre as responsabilidades de cada participante;
- alterações de status sem critérios consistentes;
- dificuldade para consultar o histórico das decisões e ações realizadas.

## Objetivo

A solução tem como objetivo **centralizar o cadastro e o acompanhamento das cargas químicas**, organizando as etapas de documentação, inspeção e avaliação técnica. O sistema deverá aplicar as regras definidas para o fluxo, controlar o acesso às funcionalidades e registrar as ações relevantes para garantir a rastreabilidade.

## Usuários envolvidos

| Usuário | Responsabilidade principal |
|---|---|
| **Embarcador** | Registrar a carga e enviar a documentação solicitada. |
| **Analista** | Avaliar a documentação e emitir o parecer documental. |
| **Fiscal** | Realizar a inspeção física e emitir o laudo de inspeção. |
| **Responsável Técnico** | Avaliar os resultados do processo e emitir o parecer técnico da carga. |
| **Operador Portuário** | Acompanhar o fluxo operacional e a situação das cargas. |
| **Gestor Operacional** | Supervisionar as operações e consultar informações gerenciais e de auditoria. |
| **Administrador do Sistema** | Gerenciar usuários, perfis e permissões de acesso. |

As permissões específicas de cada usuário serão detalhadas nos casos de uso e nas regras de negócio.

## Fluxo principal

De forma resumida, o processo previsto é:

1. o produto químico e seus requisitos são cadastrados;
2. o embarcador registra a carga, associa os produtos e envia a documentação solicitada;
3. a documentação é analisada e recebe um parecer;
4. as inspeções necessárias são realizadas e registradas;
5. o responsável técnico avalia os resultados e emite o parecer técnico;
6. a decisão sobre a movimentação da carga é registrada;
7. o histórico permanece disponível para acompanhamento e auditoria.

Pendências documentais ou de inspeção devem impedir o avanço da carga até que sejam tratadas conforme as regras do domínio.

## Escopo e informações controladas

| Área | Responsabilidade do sistema |
|---|---|
| **Produtos Químicos** | Manter identificação, classificação de risco, estado físico, situação cadastral e requisitos associados. |
| **Cargas Químicas** | Registrar produto, quantidade, embarcador, responsável técnico, datas, pendências e status. |
| **Documentação** | Controlar documentos obrigatórios, arquivos enviados e pareceres documentais. |
| **Inspeções** | Registrar inspeções exigidas, resultados e laudos. |
| **Avaliação Técnica** | Registrar o parecer técnico e a decisão sobre a movimentação da carga. |
| **Acesso** | Gerenciar usuários, perfis e permissões. |
| **Auditoria** | Manter o histórico das ações realizadas. |

Nesta primeira fase, o projeto está concentrado na compreensão do domínio e no planejamento da solução. A implementação da aplicação será realizada **nas etapas posteriores do projeto**.

## Decisões, riscos e restrições

O sistema não substitui as decisões dos profissionais responsáveis. Seu papel é validar requisitos, organizar informações e registrar as decisões tomadas pelos usuários autorizados.

O Quimovia deverá verificar:

- os documentos e as inspeções exigidos para cada produto;
- as ações que cada perfil está autorizado a executar;
- as informações necessárias para que uma carga avance no fluxo;
- as transições de status permitidas;
- as pendências que devem ser resolvidas antes da decisão técnica.

Além disso:

- apenas usuários autenticados e ativos poderão acessar o sistema;
- informações e documentos deverão ser protegidos contra acesso ou alteração indevidos;
- ações relevantes deverão gerar registros de auditoria;
- documentos e inspeções obrigatórios deverão ser concluídos antes da decisão sobre a movimentação da carga;
- as regras regulatórias utilizadas possuem finalidade acadêmica e deverão ser validadas por especialistas antes de uma eventual utilização em ambiente real.

## Evolução prevista

Nas próximas fases, o Quimovia poderá evoluir para uma **aplicação web responsiva**, com área administrativa, armazenamento de documentos, notificações e integrações externas. Também poderá ser desenvolvida uma **aplicação móvel** para consultas e atividades realizadas no local da operação.

A evolução para microsserviços deverá ser avaliada somente se o crescimento e as necessidades da solução justificarem essa complexidade.
