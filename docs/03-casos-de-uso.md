# Casos de uso

Os casos de uso descrevem as interações funcionais do **Quimovia** (sistema de gestão de cargas químicas portuárias), mapeando os objetivos dos atores, as entradas fornecidas, os resultados gerados, as regras de negócio aplicadas e os cenários de exceção.

A estrutura dos casos de uso está alinhada aos contextos delimitados na [modelagem com Domain-Driven Design](docs/02-modelagem-com-ddd.md) e às diretrizes especificadas nas [regras de negócio](docs/04-regras-de-negócio.md).

---

## Mapeamento dos casos de uso por contexto

Os casos de uso estão organizados nos cinco contextos delimitados na arquitetura do projeto:

1. [Produtos Químicos](#1-contexto-produtos-químicos)
2. [Gestão de Cargas Químicas](#2-contexto-gestão-de-cargas-químicas)
3. [Conformidade Documental](#3-contexto-conformidade-documental)
4. [Inspeções de Segurança](#4-contexto-inspeções-de-segurança)
5. [Identidade, Acesso e Operações](#5-contexto-identidade-acesso-e-operações)

---

## 1. Contexto: Produtos Químicos

### UC01: Cadastrar Produto Químico

- **Objetivo:** Registrar um novo produto químico no catálogo do sistema, definindo suas características físicas, classificação de risco e requisitos obrigatórios de documentos e inspeções.
- **Ator envolvido:** Responsável Técnico.
- **Entrada esperada:** Nome técnico, código interno único, número ONU, classe de risco, subclasse de risco (se houver), estado físico (sólido, líquido, gasoso), lista de documentos obrigatórios exigidos e lista de testes/inspeções de segurança necessários.
- **Saída esperada:** Produto químico cadastrado com status inicial **Ativo** e registro correspondente na trilha de auditoria.
- **Principais regras de negócio:**
  - Código do produto deve ser único no sistema.
  - Nome técnico, número ONU, classe de risco e estado físico são de preenchimento obrigatório.
  - Formato do número ONU e classe de risco deve atender aos padrões regulatórios.
  - Apenas o Responsável Técnico pode cadastrar novos produtos.
- **Possíveis erros ou exceções:**
  - *Código duplicado:* O sistema rejeita o cadastro e informa que o código já pertence a outro produto.
  - *Dados obrigatórios ausentes ou inválidos:* O sistema bloqueia a gravação e exibe mensagem de erro de validação nos campos (ex.: classe de risco inválida).
  - *Acesso não autorizado:* Tentativa por usuário sem o perfil de Responsável Técnico retorna erro de permissão.

---

### UC02: Alterar Status e Atualizar Produto Químico

- **Objetivo:** Atualizar os dados cadastrais ou alterar a situação (Ativo, Inativo, Bloqueado) de um produto químico no catálogo.
- **Ator envolvido:** Responsável Técnico (atualização de dados e status) / Operador Portuário (alteração de status operacional).
- **Entrada esperada:** Identificador do produto químico, novos dados cadastrais (caso seja edição) ou novo status desejado (*Ativo*, *Inativo*, *Bloqueado*) acompanhado da justificativa.
- **Saída esperada:** Dados do produto atualizados ou status alterado, com registro de auditoria indicando a alteração e a justificativa.
- **Principais regras de negócio:**
  - A inativação ou bloqueio de um produto químico impede que ele seja associado a **novas** cargas químicas.
  - Cargas já registradas que contêm o produto inativado preservam seus vínculos históricos.
  - Alterações nos requisitos de documentação do produto não modificam retroativamente análises documentais já concluídas.
- **Possíveis erros ou exceções:**
  - *Produto não encontrado:* Identificador informado não existe na base de dados.
  - *Justificativa ausente:* Tentar bloquear ou inativar sem informar a justificativa gera erro de validação.
  - *Tentativa de uso de produto inativo:* Tentar vincular o produto inativo a uma nova carga gera erro de validação no fluxo de registro de carga.

---

## 2. Contexto: Gestão de Cargas Químicas

### UC03: Registrar Carga Química

- **Objetivo:** Registrar a entrada de uma nova carga química no porto, vinculando-a a um produto químico ativo do catálogo e informando quantidades e responsável.
- **Ator envolvido:** Embarcador.
- **Entrada esperada:** Identificador do produto químico, quantidade transportada, unidade de medida (toneladas, litros, kg), identificação da remessa/lote, dados do embarcador e localização/porão/pátio inicial.
- **Saída esperada:** Carga química registrada com identificador único (ID), status inicial **Registrada** (ou **Em Análise Documental**) e lista de requisitos de documentos/inspeções herdados do produto.
- **Principais regras de negócio:**
  - O produto químico associado deve estar obrigatoriamente **Ativo** no catálogo.
  - A quantidade transportada deve ser estritamente maior que zero.
  - A carga química deve possuir uma classificação de risco associada (herdada ou especificada conforme o produto).
  - Uma carga sem produto químico associado não pode ser registrada.
- **Possíveis erros ou exceções:**
  - *Produto químico inativo ou bloqueado:* O sistema recusa o registro e alerta que o produto não está disponível para novas operações.
  - *Quantidade menor ou igual a zero:* Erro de validação de quantidade.
  - *Produto inexistente:* Identificador do produto inválido.

---

### UC04: Atribuir Responsável Técnico à Carga

- **Objetivo:** Definir o Responsável Técnico encarregado de acompanhar o processo de análise e emitir a decisão técnica final sobre a carga.
- **Ator envolvido:** Gestor Operacional.
- **Entrada esperada:** Identificador da carga química e identificador do usuário com perfil de Responsável Técnico.
- **Saída esperada:** Carga química atualizada com o Responsável Técnico vinculado e log de auditoria da atribuição.
- **Principais regras de negócio:**
  - O usuário atribuído deve ter o perfil ativo de Responsável Técnico no sistema.
  - A atribuição deve ocorrer antes do início da avaliação técnica final da carga.
- **Possíveis erros ou exceções:**
  - *Usuário com perfil incompatível ou inativo:* O sistema impede o vínculo e exibe mensagem de erro.
  - *Carga em status final (Aprovada/Reprovada/Cancelada):* Não é permitida a alteração do responsável após o encerramento do ciclo.

---

### UC05: Atualizar Status e Transição da Carga

- **Objetivo:** Avançar ou atualizar o estado da carga química ao longo do seu fluxo operacional (Registrada -> Em análise documental -> Em inspeção -> Em avaliação técnica -> Aprovada/Reprovada -> Liberada).
- **Ator envolvido:** Operador Portuário / Gestor Operacional.
- **Entrada esperada:** Identificador da carga química e o novo status pretendido.
- **Saída esperada:** Status da carga atualizado e registro de histórico de transição.
- **Principais regras de negócio:**
  - A transição de status deve respeitar rigorosamente a sequência de dependências do fluxo do domínio.
  - Uma carga não pode avançar para **Em Inspeção** sem que a análise documental esteja **Conforme**.
  - Uma carga não pode avançar para **Em Avaliação Técnica** sem que todas as inspeções obrigatórias estejam concluídas.
  - Não é permitido ignorar etapas ou executar transições fora de ordem.
- **Possíveis erros ou exceções:**
  - *Transição inválida ou violação de pré-requisito:* O sistema bloqueia a alteração e exibe as pendências que impedem o avanço (ex.: "Pendente Parecer Documental").

---

### UC06: Bloquear Carga Química

- **Objetivo:** Interromper preventivamente a movimentação da carga química diante da identificação de irregularidades, riscos de segurança ou pendências críticas.
- **Ator envolvido:** Gestor Operacional / Responsável Técnico / Operador Portuário.
- **Entrada esperada:** Identificador da carga química e motivo detalhado do bloqueio.
- **Saída esperada:** Carga com status alterado para **Bloqueada**, etapa de origem registrada e log de auditoria gerado.
- **Principais regras de negócio:**
  - O bloqueio pode ser aplicado em qualquer etapa do fluxo que anteceda a movimentação final.
  - Cargas bloqueadas não podem ser movimentadas ou liberadas.
  - É obrigatório registrar o motivo do bloqueio para rastreabilidade.
- **Possíveis erros ou exceções:**
  - *Motivo ausente:* O sistema exige o preenchimento da justificativa.
  - *Carga já cancelada ou liberada:* O bloqueio não pode ser aplicado em cargas encerradas.

---

### UC07: Desbloquear / Reavaliar Carga Química

- **Objetivo:** Liberar uma carga do estado de bloqueio após a resolução das causas impeditivas, retornando-a à etapa adequada do fluxo.
- **Ator envolvido:** Gestor Operacional / Responsável Técnico.
- **Entrada esperada:** Identificador da carga química, evidências/justificativa de resolução das pendências e indicação da etapa de retorno.
- **Saída esperada:** Status da carga restaurado para a etapa de reavaliação cabível e registro do desbloqueio na auditoria.
- **Principais regras de negócio:**
  - O desbloqueio exige a confirmação do tratamento da causa que originou o bloqueio.
  - A carga retorna para a etapa correspondente (ex.: reanálise documental ou nova inspeção) e não direto para liberação.
- **Possíveis erros ou exceções:**
  - *Pendências ainda não resolvidas:* O sistema impede o desbloqueio caso laudos ou pareceres pendentes não estejam regularizados.

---

### UC08: Liberar Carga Química

- **Objetivo:** Conceder a autorização operacional para a movimentação da carga química no porto após aprovação técnica.
- **Ator envolvido:** Operador Portuário / Responsável Técnico.
- **Entrada esperada:** Identificador da carga química previamente aprovada.
- **Saída esperada:** Status da carga atualizado para **Liberada**, autorização de movimentação emitida e log de auditoria.
- **Principais regras de negócio:**
  - A carga só pode ser liberada se o Parecer Técnico estiver **Favorável / Aprovada**.
  - Não é permitida a liberação de cargas com pendência documental, inspeção pendente, reprovadas ou bloqueadas.
- **Possíveis erros ou exceções:**
  - *Carga não aprovada ou com pendências:* O sistema recusa a liberação e informa os impedimentos.

---

### UC09: Cancelar Carga Química

- **Objetivo:** Encerrar definitivamente o processamento de uma carga química no sistema devido a desistência da operação ou irregularidade insanável.
- **Ator envolvido:** Gestor Operacional.
- **Entrada esperada:** Identificador da carga química e justificativa formal de cancelamento.
- **Saída esperada:** Status da carga alterado para **Cancelada**, impedindo qualquer ação futura sobre ela.
- **Principais regras de negócio:**
  - O cancelamento é definitivo e irreversível.
  - Uma carga cancelada não pode ser liberada ou reativada.
  - O registro da carga e todo o seu histórico permanecem salvos para fins de auditoria.
- **Possíveis erros ou exceções:**
  - *Carga já movimentada/liberada:* Não é permitido cancelar uma carga que já concluiu a movimentação.

---

### UC10: Consultar Cargas e Histórico de Movimentação

- **Objetivo:** Pesquisar cargas químicas registradas no sistema, filtrar por parâmetros operacionais e visualizar o histórico completo de transições, laudos e pareceres.
- **Ator envolvido:** Operador Portuário, Gestor Operacional, Responsável Técnico, Analista, Fiscal, Embarcador (suas próprias cargas).
- **Entrada esperada:** Filtros de consulta (status da carga, código do produto, período de registro, embarcador, responsável técnico, identificador da carga).
- **Saída esperada:** Lista de cargas correspondentes aos filtros e visualização detalhada da linha do tempo/histórico da carga selecionada.
- **Principais regras de negócio:**
  - Usuários visualizam apenas as informações permitidas pelo seu perfil de acesso (ex.: embarcador consulta apenas suas cargas).
  - O histórico deve exibir todas as mudanças de status, pareceres, laudos, bloqueios e identificadores dos usuários responsáveis por cada ação.
- **Possíveis erros ou exceções:**
  - *Nenhum resultado encontrado:* Sistema exibe mensagem indicando ausência de dados para os filtros informados.

---

## 3. Contexto: Conformidade Documental

### UC11: Registrar e Anexar Documentação da Carga

- **Objetivo:** Anexar e enviar os arquivos digitais dos documentos obrigatórios (ex.: FDS/FISPQ, Manifesto de Carga, Certificado de Análise) exigidos para a carga química.
- **Ator envolvido:** Embarcador.
- **Entrada esperada:** Identificador da carga química, tipo de documento (de acordo com a lista exigida pelo produto) e arquivo digital correspondente (PDF).
- **Saída esperada:** Documentos vinculados à carga, status da documentação atualizado e carga disponibilizada para análise documental.
- **Principais regras de negócio:**
  - Todos os documentos marcados como **obrigatórios** para o produto químico da carga devem ser anexados antes de submeter para análise.
  - Os arquivos devem respeitar os formatos (ex.: PDF) e tamanhos máximos permitidos pelo sistema.
  - Após o envio para análise, a documentação fica bloqueada para edição até a emissão do parecer.
- **Possíveis erros ou exceções:**
  - *Documento obrigatório faltando:* O sistema impede o envio e lista os documentos pendentes.
  - *Formato ou tamanho de arquivo inválido:* O sistema rejeita o upload e exibe mensagem informativa.

---

### UC12: Analisar e Emitir Parecer Documental

- **Objetivo:** Avaliar a conformidade da documentação enviada pelo embarcador e emitir o Parecer Documental oficial.
- **Ator envolvido:** Analista.
- **Entrada esperada:** Identificador da documentação da carga, resultado da avaliação (*Conforme* ou *Não Conforme*) e descrição de pendências/observações (obrigatório se *Não Conforme*).
- **Saída esperada:** Parecer Documental gerado com data, hora, ID do analista e resultado registrados.
- **Principais regras de negócio:**
  - Se o parecer for **Conforme**, a carga é habilitada para prosseguir para a etapa de inspeção.
  - Se o parecer for **Não Conforme**, são registradas as pendências documentais e a carga é retida ou solicitada a correção ao embarcador.
  - O Parecer Documental emitido não pode ser alterado ou apagado; reavaliações geram novos pareceres históricos.
- **Possíveis erros ou exceções:**
  - *Parecer Não Conforme sem justificativa:* O sistema exige a descrição detalhada das pendências.
  - *Tentativa de análise por perfil diferente de Analista:* Permissão negada.

---

### UC13: Consultar Parecer Documental

- **Objetivo:** Visualizar os pareceres documentais emitidos para uma carga, verificando a conformidade e os detalhes de eventuais pendências.
- **Ator envolvido:** Responsável Técnico, Gestor Operacional, Analista, Embarcador.
- **Entrada esperada:** Identificador da carga química ou da documentação.
- **Saída esperada:** Exibição do histórico de pareceres documentais, status atual da documentação e arquivos anexados.
- **Principais regras de negócio:**
  - O Responsável Técnico utiliza a consulta ao Parecer Documental como subsídio obrigatório para a tomada de decisão técnica final.
- **Possíveis erros ou exceções:**
  - *Documentação inexistente para a carga:* O sistema indica que a carga ainda não possui documentos anexados.

---

## 4. Contexto: Inspeções de Segurança

### UC14: Solicitar / Registrar Ordem de Inspeção

- **Objetivo:** Inicializar uma ordem de inspeção técnica e teste de segurança para a carga química e seus produtos.
- **Ator envolvido:** Gestor Operacional / Fiscal.
- **Entrada esperada:** Identificador da carga química, produto/item a ser inspecionado e tipo de inspeção exigida (ex.: inspeção visual de embalagem, amostragem, medição de temperatura/pressão).
- **Saída esperada:** Ordem de inspeção gerada com status **Pendente** e atribuída ao grupo de fiscais.
- **Principais regras de negócio:**
  - A solicitação de inspeção depende do Parecer Documental **Conforme**.
  - A inspeção deve cobrir todos os testes obrigatórios previstos para a classe de risco do produto.
- **Possíveis erros ou exceções:**
  - *Documentação pendente ou não conforme:* O sistema impede o agendamento da inspeção.

---

### UC15: Realizar Inspeção e Emitir Laudo de Segurança

- **Objetivo:** Registrar as verificações físicas e operacionais realizadas na carga e emitir o Laudo de Segurança correspondente.
- **Ator envolvido:** Fiscal.
- **Entrada esperada:** Identificador da ordem de inspeção, checklist de itens inspecionados (integridade de lacres, embalagem, rotulagem, prazo de validade), resultado geral (*Conforme* ou *Não Conforme*) e parecer descritivo do fiscal.
- **Saída esperada:** Laudo de Segurança registrado no sistema, vinculando a data, hora e identificação do Fiscal.
- **Principais regras de negócio:**
  - Todos os itens de checagem obrigatórios devem ser preenchidos antes de concluir o laudo.
  - Um laudo **Não Conforme** impede o avanço da carga e pode disparar o bloqueio automático da carga.
  - Laudos emitidos são imutáveis; novas checagens exigem a emissão de uma nova inspeção/reinspeção.
- **Possíveis erros ou exceções:**
  - *Checklist incompleto:* O sistema bloqueia a emissão até que todos os itens do teste sejam avaliados.

---

### UC16: Consultar Laudo de Segurança

- **Objetivo:** Consultar os laudos de segurança físicos e relatórios de inspeção emitidos para uma determinada carga química.
- **Ator envolvido:** Responsável Técnico, Gestor Operacional, Fiscal.
- **Entrada esperada:** Identificador da carga química ou da ordem de inspeção.
- **Saída esperada:** Apresentação do laudo de segurança com o detalhamento das verificações físicas e apontamentos do fiscal.
- **Principais regras de negócio:**
  - O Laudo de Segurança Conforme é requisito indispensável para a emissão do Parecer Técnico Favorável pelo Responsável Técnico.
- **Possíveis erros ou exceções:**
  - *Laudo não encontrado:* A carga ainda não passou por inspeção técnica.

---

## 5. Contexto: Identidade, Acesso e Operações

### UC17: Autenticar Usuário (Login)

- **Objetivo:** Validar a identidade de um usuário no sistema e criar uma sessão de acesso segura.
- **Ator envolvido:** Usuário do Sistema (qualquer perfil).
- **Entrada esperada:** E-mail cadastrado e senha.
- **Saída esperada:** Token de autenticação gerado (sessão ativa), identificação do perfil do usuário e redirecionamento para o painel correspondente.
- **Principais regras de negócio:**
  - Apenas usuários com status **Ativo** podem realizar login no sistema.
  - A senha informada deve corresponder ao hash armazenado na base de dados.
  - A sessão gerada possui prazo de expiração predefinido.
- **Possíveis erros ou exceções:**
  - *Credenciais inválidas:* O sistema exibe mensagem de erro de autenticação sem especificar se o e-mail ou a senha está incorreto.
  - *Usuário inativo ou bloqueado:* O sistema recusa o acesso e informa que a conta está desativada.

---

### UC18: Gerenciar Usuários, Perfis e Permissões

- **Objetivo:** Cadastrar novos usuários, editar perfis de acesso (Embarcador, Analista, Fiscal, Responsável Técnico, Operador Portuário, Gestor, Administrador) e gerenciar permissões.
- **Ator envolvido:** Administrador do Sistema.
- **Entrada esperada:** Nome do usuário, e-mail único, perfil atribuído, status (Ativo/Inativo) e definições de permissões específicas.
- **Saída esperada:** Usuário ou perfil cadastrado/atualizado com log de auditoria da alteração.
- **Principais regras de negócio:**
  - O e-mail do usuário deve ser único em todo o sistema.
  - Todo usuário deve possuir obrigatoriamente um perfil de acesso associado.
  - As senhas cadastradas devem atender aos critérios mínimos de complexidade e segurança.
- **Possíveis erros ou exceções:**
  - *E-mail duplicado:* O sistema recusa o cadastro e indica que o e-mail já está em uso.
  - *Perfil inexistente:* Erro na atribuição de função inválida.

---

## Diagramas de casos de uso associados

Os diagramas de casos de uso que detalham as interações visuais e fluxos por subdomínio encontram-se disponíveis na documentação do projeto:

- **Usuários e Operações:** [docs/usuarios-operacoes/03-casos-de-uso.md](docs/usuarios-operacoes/03-casos-de-uso.md)
- **Produtos Químicos:** [docs/produtos-quimicos/03-casos-de-uso.md](docs/produtos-quimicos/03-casos-de-uso.md)
- **Cargas Químicas:** [docs/cargas-quimicas/03-casos-de-uso.md](docs/cargas-quimicas/03-casos-de-uso.md)
- **Conformidade Documental:** [docs/documentacoes-cargas-produtos/03-casos-de-uso.md](docs/documentacoes-cargas-produtos/03-casos-de-uso.md)
- **Inspeções de Segurança:** [docs/inspecoes/03-casos-de-uso.md](docs/inspecoes/03-casos-de-uso.md)
- **Consolidação de Diagramas:** [docs/07-diagramas.md](docs/07-diagramas.md)

---

## Matriz de Rastreabilidade: Atores vs. Casos de Uso

| Caso de Uso | Embarcador | Analista | Fiscal | Resp. Técnico | Operador Portuário | Gestor Operacional | Admin |
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **UC01: Cadastrar Produto Químico** | | | | X | | | |
| **UC02: Alterar Status de Produto** | | | | X | X | | |
| **UC03: Registrar Carga Química** | X | | | | | | |
| **UC04: Atribuir Resp. Técnico** | | | | | | X | |
| **UC05: Atualizar Status da Carga** | | | | | X | X | |
| **UC06: Bloquear Carga Química** | | | | X | X | X | |
| **UC07: Desbloquear / Reavaliar** | | | | X | | X | |
| **UC08: Liberar Carga Química** | | | | X | X | | |
| **UC09: Cancelar Carga Química** | | | | | | X | |
| **UC10: Consultar Cargas e Histórico** | X | X | X | X | X | X | X |
| **UC11: Registrar Documentação** | X | | | | | | |
| **UC12: Analisar Documentação** | | X | | | | | |
| **UC13: Consultar Parecer Documental**| X | X | | X | | X | |
| **UC14: Solicitar Inspeção** | | | X | | | X | |
| **UC15: Realizar Inspeção e Laudo** | | | X | | | | |
| **UC16: Consultar Laudo de Segurança**| | | X | X | | X | |
| **UC17: Autenticar Usuário (Login)** | X | X | X | X | X | X | X |
| **UC18: Gerenciar Usuários/Perfis** | | | | | | | X |

---

## Documentos correlatos

- [docs/01-entendimento-do-dominio.md](docs/01-entendimento-do-dominio.md)
- [docs/02-modelagem-com-ddd.md](docs/02-modelagem-com-ddd.md)
- [docs/04-regras-de-negócio.md](docs/04-regras-de-negócio.md)
- [docs/05-arquitetura-proposta.md](docs/05-arquitetura-proposta.md)
- [docs/07-diagramas.md](docs/07-diagramas.md)
- [docs/08-planejamento-de-qualidade-de-software.md](docs/08-planejamento-de-qualidade-de-software.md)

