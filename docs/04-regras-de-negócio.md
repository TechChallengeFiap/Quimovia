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
## Lotes dos Produtos:
- RECEIVED – Recebido e registrado, mas ainda não passou por todas as conferências. Não pode ser movimentado.
- UNDERINSPECTION – Lote está sendo inspecionado. Não pode ser movimentado.
- AVAILABLE – Lote aprovado e disponível para operações. Pode ser movimentado.
- RESERVED – Lote reservado para uma operação específica. Só pode ser movimentado para operação que reservou.
- QUARANTINED – O lote foi isolado por suspeita de problema, não conformidade ou exigência legal. Não pode ser movimentado.
- RELEASED – O lote saiu definitivamente do porto. Não pode ser movimentado.
- EXPIRED – O lote venceu o prazo de validade. Não pode ser movimentado.
- DISPOSED – O lote foi descartado ou destruído conforme normas ambientais. Não pode ser movimentado.
----
## Usuários:
- Um usuário deve ter um e-mail único.
- Um usuário deve ter um perfil definido.
- Somente um usuário com status ativo pode autenticar-se no sistema.
- A senha do usuário deve atender aos critérios mínimos de segurança definidos pelo sistema.
