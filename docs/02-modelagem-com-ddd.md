# Modelagem com Domain Driven Design
Nesse documento, iremos apresentar as modelagens divididos por domínios, no qual iremos te redirecionar para cada documento de cada domínio, para que você possa entender melhor cada modelagem.

## Domínios:
- ### [Cargas Químicas](cargas-quimicas/02-modelagem-com-ddd.md)
- ### [Documentações de Cargas e Produtos](documentacoes-cargas-produtos/02-modelagem-com-ddd.md)
- ### [Produtos Químicos](produtos-quimicos/02-modelagem-com-ddd.md)
- ### [Usuários e Operações](usuarios-operacoes/02-modelagem-com-ddd.md)


## Linguagem Ubíqua
Na tabela abaixo esta listado os termos e significados que serão utilizados no projeto, com o objetivo de criar uma linguagem comum entre todos os envolvidos no projeto e aos leitores, garantindo que todos compreendam os termos e conceitos utilizados no sistema.

| Termo | Significado |
|-------|-------------|
| **Administrador do Sistema** | Usuário responsável por gerenciar os usuários e perfis do sistema, garantindo que cada usuário tenha as permissões corretas de acordo com seu perfil. |
| **Analista** | Usuário responsável por validar a documentação e emitir o Parecer Documental. |
| **Carga Química** | Mercadoria contendo produtos químicos que será transportada no porto. Após apresentação das documentações e inspeções obrigatórias dessa carga, será decidida a liberação ou não da carga. |
| **Classificação de Risco** | Avaliação do risco associado a cada carga química, considerando fatores como toxicidade, inflamabilidade, reatividade e outros. |
| **Documento Anexado** | Arquivo enviado pelo embarcador como parte da documentação da carga, utilizado para comprovar o atendimento às exigências legais, regulatórias e operacionais. |
| **Documentação da Carga** | Conjunto de informações e documentos enviados pelo embarcador para uma carga química. |
| **Documentação Não Conforme** | Documentação que apresenta inconsistências ou pendências e exige correção antes da continuidade do processo. |
| **Documentação Obrigatória** | Conjunto de documentos que devem ser apresentados para cada carga química, incluindo certificados de análise, fichas de segurança, autorizações e outros. |
| **Documentação Conforme** | Documentação que atende aos requisitos exigidos e pode seguir para a etapa de inspeção. |
| **Embarcador** | Usuário externo responsável pela chegada das novas cargas químicas no porto. Ele irá registrar as cargas e produtos da carga, além de enviar a documentação e inspeções obrigatórias para cada carga. |
| **Fiscal** | Usuário responsável por realizar a inspeção física da carga e emitir o Laudo de Inspeção. Também é responsável por cadastrar as inspeções obrigatórias para cada produto químico. |
| **Gestor Operacional** | Usuário responsável por acompanhar e gerenciar o status das cargas químicas, e também responsável pela auditoria do sistema, garantindo que todas as informações estejam corretas e atualizadas. |
| **Operador Portuário** | Usuário responsável pelo acompanhamento das cargas químicas no porto, alterando status, validando documentos e inspeções, garantindo que todas as informações estejam corretas e atualizadas. |
| **Parecer Documental** | Resultado da análise realizada pelo Analista sobre a documentação apresentada. |
| **Parecer Técnico da Carga** | Documento emitido pelo Responsável Técnico que registra a decisão final sobre a movimentação da carga com base nas análises documental e de inspeção. |
| **Produto Químico** | Substância química que será transportada no porto, podendo ser perigosa ou não, e que possui documentações e inspeções obrigatórias. |
| **Responsável Técnico** | Usuário responsável pelo cadastro de novos produtos químicos, documentações obrigatórias e inspeções obrigatórias para cada produto químico, e também responsável pela aprovação ou reprovação de cargas químicas. |
| **Status da Carga** | Indicação do estado atual da carga química, podendo ser "Em Análise", "Aprovado", "Reprovado" ou outros status definidos pelo sistema. |
| **Inspeção** | Conjunto de inspeções que devem ser realizadas em cada carga química para garantir que ela atende aos padrões de qualidade e segurança exigidos pelas regulamentações. |
| **Usuário** | Pessoa que interage com o sistema, podendo ter diferentes perfis e permissões de acesso, como Operador Portuário, Embarcador, Analista, Fiscal, Administrador do Sistema e Gestor Operacional. |
