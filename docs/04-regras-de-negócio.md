# Regras de negócio
Nesse documento juntamos as regras de cada entidades por domínio, para que você possa entender melhor cada regra de negócio do sistema.

## Regras: 
### Cargas químicas:
- Uma **carga química** deve ter um produto químico associado.
- Uma **carga química** deve ter um status definido.
- Uma **carga química** deve ter um responsável técnico definido.
- Uma **carga química** deve ter um embarcador definido.
- Uma **carga química** deve ter uma documentação associada.
----
## Documentação da Carga:
- Toda documentação deve estar vinculada a uma carga química.
- Toda documentação deve possuir pelo menos um documento anexado.
- Apenas o embarcador responsável pode adicionar, remover ou substituir documentos.
- Após o início da análise documental, a documentação permanece bloqueada para alterações.
- Apenas usuários com perfil de Analista podem emitir pareceres documentais.
- Todo parecer deve possuir um resultado definido.
- Pareceres classificados como **Não Conforme** exigem justificativa.
- Um parecer somente pode ser emitido após o envio da documentação.
----
## Produtos Químicos:
- Todo produto químico deve possuir um Código único.
- Todo produto químico deve possuir um Nome Técnico.
- Todo produto químico deve possuir um Número ONU.
- Todo produto químico deve possuir uma Classe de Risco.
- Todo produto químico deve possuir um Estado Físico.
- Apenas produtos ativos podem ser utilizados em novas operações.
----
## Usuários:
- Um usuário deve ter um e-mail único.
- Um usuário deve ter um perfil definido.
- Somente um usuário com status ativo pode autenticar-se no sistema.
- A senha do usuário deve atender aos critérios mínimos de segurança definidos pelo sistema.