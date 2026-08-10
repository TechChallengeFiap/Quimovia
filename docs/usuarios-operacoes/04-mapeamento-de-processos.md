# Mapeamento de Processos — Usuários e Operações

Este documento apresenta o mapeamento dos processos (fluxogramas) do domínio **Usuários e Operações**, detalhando como cada entidade se comunica, quais decisões o sistema toma e quais regras de negócio são validadas em cada etapa.

O objetivo é servir de apoio visual à [Modelagem com DDD](02-modelagem-com-ddd.md) e aos [Casos de Uso](03-casos-de-uso.md) já documentados, deixando explícito o **fluxo** por trás de cada regra descrita em [Regras de Negócio](../04-regras-de-negócio.md).

## Legenda

| Símbolo (Mermaid) | Significado |
|---|---|
| `([Texto])` | Início / Fim do processo |
| `[Texto]` | Atividade / Ação do sistema ou ator |
| `{Texto}` | Decisão / Validação de regra de negócio |
| `[(Texto)]` | Persistência (leitura ou gravação em banco) |
| Subgraph (raia) | Ator responsável pela etapa (Administrador, Usuário, Sistema) |

---

## 1. Fluxo: Cadastro de Usuário

**Ator principal:** Administrador do Sistema
**Regras aplicadas:** e-mail único, perfil obrigatório, senha com critérios mínimos de segurança.

```mermaid
flowchart TD
    subgraph ADM["Administrador do Sistema"]
        A1(["Início: Cadastrar novo usuário"])
        A2["Informa nome, e-mail, senha e perfil"]
    end

    subgraph SYS["Sistema"]
        B1{"E-mail já cadastrado?"}
        B2{"Perfil informado existe?"}
        B3{"Senha atende critérios mínimos?"}
        B4["Gera hash da senha"]
        B5["Define status = ATIVO"]
        B6[("Persiste Usuário")]
        B7(["Fim: Usuário cadastrado com sucesso"])
        E1["Retorna erro: e-mail duplicado"]
        E2["Retorna erro: perfil inválido"]
        E3["Retorna erro: senha fora dos critérios"]
        E4(["Fim: Cadastro rejeitado"])
    end

    A1 --> A2 --> B1
    B1 -- Sim --> E1 --> E4
    B1 -- Não --> B2
    B2 -- Não --> E2 --> E4
    B2 -- Sim --> B3
    B3 -- Não --> E3 --> E4
    B3 -- Sim --> B4 --> B5 --> B6 --> B7
```

---

## 2. Fluxo: Autenticação (Login)

**Ator principal:** Usuário do Sistema (qualquer perfil)
**Regras aplicadas:** somente usuário ativo autentica; sessão só é criada para usuário válido e ativo.

```mermaid
flowchart TD
    subgraph USR["Usuário"]
        A1(["Início: Solicita login"])
        A2["Informa e-mail e senha"]
    end

    subgraph SYS["Sistema"]
        B1[("Busca Usuário pelo e-mail")]
        B2{"Usuário encontrado?"}
        B3{"Status = ATIVO?"}
        B4{"Senha corresponde ao hash?"}
        B5["Cria Sessão (token, criadaEm, expiraEm)"]
        B6["Atualiza ultimoLogin do Usuário"]
        B7(["Fim: Login realizado, sessão ativa"])
        E1["Retorna erro: credenciais inválidas"]
        E2["Retorna erro: usuário inativo ou bloqueado"]
        E3(["Fim: Login rejeitado"])
    end

    A1 --> A2 --> B1 --> B2
    B2 -- Não --> E1 --> E3
    B2 -- Sim --> B3
    B3 -- Não --> E2 --> E3
    B3 -- Sim --> B4
    B4 -- Não --> E1
    B4 -- Sim --> B5 --> B6 --> B7
```

---

## 3. Fluxo: Gestão de Sessão (Renovação e Encerramento)

**Ator principal:** Sistema (acionado por Usuário ou por expiração)
**Regras aplicadas:** sessão expirada não pode ser reutilizada, apenas renovada por novo login; encerrar uma sessão não afeta as demais sessões ativas do mesmo usuário.

```mermaid
flowchart TD
    A1(["Início: Requisição autenticada recebida"])
    B1[("Consulta Sessão pelo token")]
    B2{"Sessão existe?"}
    B3{"Sessão expirada?"}
    B4["Processa requisição normalmente"]
    B5(["Fim: Requisição processada"])
    E1["Retorna erro: sessão inválida"]
    E2["Retorna erro 401: sessão expirada, novo login necessário"]
    E3(["Fim: Acesso negado"])
    C1["Usuário solicita encerrar sessão"]
    C2["Encerra apenas a sessão informada"]
    C3(["Fim: Sessão encerrada, demais sessões do usuário permanecem ativas"])

    A1 --> B1 --> B2
    B2 -- Não --> E1 --> E3
    B2 -- Sim --> B3
    B3 -- Sim --> E2 --> E3
    B3 -- Não --> B4 --> B5

    C1 --> C2 --> C3
```

---

## 4. Fluxo: Gerenciamento de Perfis e Permissões

**Ator principal:** Administrador do Sistema
**Regras aplicadas:** nome do perfil único, perfil deve ter ao menos uma permissão, permissões não podem se repetir (mesmo recurso + ação) em um perfil.

```mermaid
flowchart TD
    subgraph ADM["Administrador do Sistema"]
        A1(["Início: Criar/editar Perfil"])
        A2["Informa nome do perfil e permissões (recurso + ação)"]
    end

    subgraph SYS["Sistema"]
        B1{"Nome do perfil já existe?"}
        B2{"Ao menos 1 permissão informada?"}
        B3{"Há permissão duplicada (recurso + ação)?"}
        B4[("Persiste Perfil/Cargo")]
        B5(["Fim: Perfil salvo com sucesso"])
        E1["Retorna erro: nome de perfil duplicado"]
        E2["Retorna erro: perfil precisa de ao menos uma permissão"]
        E3["Retorna erro: permissão duplicada no perfil"]
        E4(["Fim: Operação rejeitada"])
    end

    A1 --> A2 --> B1
    B1 -- Sim --> E1 --> E4
    B1 -- Não --> B2
    B2 -- Não --> E2 --> E4
    B2 -- Sim --> B3
    B3 -- Sim --> E3 --> E4
    B3 -- Não --> B4 --> B5
```

---

## 5. Fluxo: Bloqueio, Desbloqueio e Desativação de Usuário

**Ator principal:** Administrador do Sistema
**Regras aplicadas:** status alterado apenas via entidade Usuário; usuário bloqueado não pode ser reativado automaticamente — exige ação do Administrador.

```mermaid
flowchart TD
    subgraph ADM["Administrador do Sistema"]
        A1(["Início: Ação sobre status do usuário"])
        A2{"Ação desejada"}
    end

    subgraph SYS["Sistema"]
        B1{"Usuário existe?"}
        B2["Altera status para BLOQUEADO"]
        B3["Encerra todas as sessões ativas do usuário"]
        B4["Altera status para INATIVO"]
        B5["Administrador confirma reativação manual"]
        B6["Altera status para ATIVO"]
        B7[("Persiste alteração de status")]
        B8(["Fim: Status atualizado"])
        E1["Retorna erro: usuário não encontrado"]
        E2(["Fim: Operação rejeitada"])
    end

    A1 --> A2
    A2 -- Bloquear --> B1
    A2 -- Desativar --> B1
    A2 -- Reativar (usuário bloqueado) --> B1

    B1 -- Não --> E1 --> E2
    B1 -- Sim --> C{"Qual ação?"}
    C -- Bloquear --> B2 --> B3 --> B7 --> B8
    C -- Desativar --> B4 --> B7
    C -- Reativar --> B5 --> B6 --> B7
```

---

## Rastreabilidade: Regra de Negócio × Processo

| Regra de Negócio | Processo onde é validada | Camada responsável (proposta) |
|---|---|---|
| Um usuário deve ter e-mail único | Cadastro de Usuário (Fluxo 1) | Domain (Agregado Usuário) |
| Um usuário deve ter um perfil definido | Cadastro de Usuário (Fluxo 1) | Domain (Agregado Usuário) |
| A senha deve atender aos critérios mínimos de segurança | Cadastro de Usuário (Fluxo 1) | Application (Serviço de Cadastro) |
| Somente usuário ativo pode autenticar-se | Autenticação (Fluxo 2) | Domain (Entidade Usuário) |
| Sessão só é criada para usuário válido e ativo | Autenticação (Fluxo 2) | Domain (Agregado Usuário) |
| Sessão expirada não pode ser reutilizada, apenas renovada | Gestão de Sessão (Fluxo 3) | Domain (Entidade Sessão) |
| Encerrar uma sessão não afeta as demais sessões ativas | Gestão de Sessão (Fluxo 3) | Domain (Entidade Sessão) |
| Todo perfil deve ter nome único | Gerenciamento de Perfis (Fluxo 4) | Domain (Entidade Perfil/Cargo) |
| Perfil deve possuir ao menos uma permissão associada | Gerenciamento de Perfis (Fluxo 4) | Domain (Entidade Perfil/Cargo) |
| Perfil não pode ter permissões duplicadas para o mesmo recurso e ação | Gerenciamento de Perfis (Fluxo 4) | Domain (Objeto de Valor Permissão) |
| Status do usuário só pode ser alterado pela entidade Usuário | Bloqueio/Desbloqueio (Fluxo 5) | Domain (Entidade Usuário) |
| Usuário bloqueado não é reativado automaticamente, exige ação do Administrador | Bloqueio/Desbloqueio (Fluxo 5) | Application (Serviço de Gestão de Acesso) |

---

## Próximos passos

- Validar com o restante do grupo se os fluxos de **Autenticação** e **Gestão de Sessão** cobrem também os requisitos de auditoria (log de login/logout), pendente de definição em conjunto com o domínio de Documentação/Auditoria.
- Alinhar com o domínio **Cargas Químicas** o ponto exato onde a permissão do usuário é consultada antes de ações como aprovar/bloquear carga (dependência entre domínios).
