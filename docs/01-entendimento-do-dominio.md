# Entendimento do Domínio
### Contexto do projeto: 
O projeto tem como objetivo desenvolver um sistema de **gestão de cargas químicas** no porto de Santos, organizando informações necessárias para que a entrada de cargas químicas seja feita de forma segura, com a documentação e testes de segurança/qualidade necessários, garantindo a conformidade com regulamentações e eficiência operacional.

Dentro desse sistema, teremos cadastros de produtos químicos, registro de cargas, classificação de risco, controle de documentação obrigatória, acompanhamento de testes de qualidade e segurança, acompanhamento do status da carga, gestão de usuários/perfis e relatórios (logs) para auditoria.

## Qual o problema o sistema pretende resolver?
O sistema pretende resolver a dificuldade de controlar cargas por meio de processos manuais ou informações espalhadas. Otimizar o processo de controle de documentação, testes e status das cargas químicas, garantindo que todas as informações estejam centralizadas e acessíveis.

## Quem são os usuários envolvidos?
- Operador Portuário
- Embarcador
- Analista
- Fiscal
- Administrador do sistema
- Gestor Operacional
- Responsável Técnico

## Quais informações precisam ser controladas?
- Produtos químicos
- Cargas químicas
- Classificação de risco
- Documentação obrigatória
- Testes de qualidade e segurança
- Status da carga
- Usuários e perfis
- Logs e relatórios para auditoria

## Quais processos fazem parte da operação?
- Cadastro de produtos químicos
- Registro de cargas químicas
- Registro de classificação de risco
- Registro de documentação obrigatória
- registro de testes de qualidade e segurança
- Definição de responsáveis técnicos
- Gerenciamento de usuários e perfis
- Gerenciamento de logs e relatórios para auditoria
- Gerenciamento de status da carga

## Quais decisões precisam ser tomadas pelo sistema?
- Definição de responsáveis técnicos para cada carga
- Definição de testes de qualidade e segurança obrigatórios para cada carga
- Definição de documentação obrigatória para cada carga
- Definição de classificação de risco para cada carga
- Definição de status da carga (em análise, aprovado, reprovado, em trânsito, entregue, etc.)
- Definição de perfis de usuários e permissões de acesso
- Definição de relatórios e logs para auditoria

## Quais riscos ou restrições precisam ser considerados?
- Garantir as permissões de acesso corretas para cada perfil de usuário
- Garantir a integridade e segurança das informações armazenadas no sistema
- Registro de todas as ações realizadas no sistema para auditoria
- Garantir que todas as cargas químicas estejam em conformidade com regulamentações e normas de segurança

## Quais partes do sistema poderão evoluir nas próximas fases?
Pensamos em evoluir para as próximas fases do projeto criar uma plataforma web responsiva que permita o acesso remoto e em tempo real que os usuários ja possam registrar cargas, documentos e testes de qualidade e segurança.

Uma área administrativa para que os gestores possam acompanhar o status das cargas, relatórios e logs de auditoria, além de permitir a gestão de usuários, perfis e produtos químicos.

Futuramente criaremos o app mobile para que os operadores portuários possam registrar cargas, documentos e testes de qualidade e segurança diretamente do local de operação, garantindo maior agilidade e eficiência no processo.

E por fim, iremos evoluir nosso backend para microsserviços, permitindo maior escalabilidade e flexibilidade no desenvolvimento e manutenção do sistema.