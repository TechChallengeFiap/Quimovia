# Diagramas
Para facilitar a compreensão da solução proposta, primeiro iremos apresentar os dois diagramas necessários do tech challenge e em sequência os diagramas de casos de uso que fizemos para cada dominio para explicar os comportamentos de cada um.

## Fluxo de transição de status da carga:
```mermaid
---
config:
  theme: redux
---
flowchart TB
    A(["Start"]) -- Embarcador: Registra Carga e Documentações --> B["Em_Analise"]
    B --> C{"Valida Documentaçao"}
    C --> D["Em_Inspecao"]
    C --> E["Reprovada"]
    C --> F["Bloqueada"]
    F --Atualiza documentação--> B
    D --> G{"Valida Segurança e Qualidade"}
    H --> E
    G --> E
    G --> H["Liberada"]
    H --Aprovação concedida--> I["Aprovada"]
    I --> J(["End"])
    E --> J
```

## Diagrama de Domínio
![Diagrama de domínio](../diagrams/Diagrama%20de%20Domínio.png)

## Opcionais:
Tivemos diversos diagramas de casos de uso que fizemos para cada domínio, e explicamos cada um deles quando estavamos apresentando a modelagem e os casos de uso. Mas tivemos alguns fluxogramas para usuário para explicar melhor o funcionamento de cadastro e gerenciamento de perfis de usuário.

## Mapeamento de Processos — Usuários e Operações

### 1. Fluxo: Cadastro de Usuário

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

### 2. Fluxo: Autenticação (Login)

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

### 3. Fluxo: Gestão de Sessão (Renovação e Encerramento)

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

### 4. Fluxo: Gerenciamento de Perfis e Permissões

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

### 5. Fluxo: Bloqueio, Desbloqueio e Desativação de Usuário

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

