# Organização do Projeto

O projeto será organizado de forma modular, separando documentação e código-fonte.

Para consultas de diagrams e afins será utilizado esse mesmo repositório.

```text
docs/
src/
tests/
README.md
```

## Docs:
Contém toda a documentação do projeto, incluindo diagramas, especificações e guias de arquitetura.

## src:
Contém todo o código-fonte do projeto, organizado em diferentes camadas de acordo com a arquitetura clean.

### domain:
Pensando em modularidade, cada módulo do domínio será separado em subdiretórios. E terão como objetivo conter apenas as entidades e regras de negócio, sem dependências externas.
#### chemical-loads:
Contém as entidades e regras de negócio relacionadas a cargas químicas.

### application:
Cada módulo do domínio terá um módulo correspondente na camada de aplicação, que será responsável por orquestrar as operações e interações entre os casos de uso e os serviços externos.
#### chemical-loads:
Contém os casos de uso e serviços relacionados a cargas químicas.

### infrastructure:
Cada módulo do domínio terá um módulo correspondente na camada de infraestrutura, que será responsável por fornecer implementações concretas para os repositórios, serviços externos e outras dependências.
#### chemical-loads:
Contém as implementações de repositórios, serviços externos e outras dependências relacionadas a cargas químicas.

### presentation:
Cada módulo do domínio terá um módulo correspondente na camada de apresentação, que será responsável por expor as APIs, endpoints e interfaces de usuário.
#### chemical-loads:
Contém os controladores, rotas e outros componentes relacionados à apresentação de cargas químicas.

### shared:
Irá conter componentes compartilhados entre diferentes módulos, como utilitários, constantes e classes base.

## tests:
Neste diretório, serão organizados os testes unitários e de integração do projeto, seguindo a mesma estrutura modular do código-fonte.
#### chemical-loads:
Contém os testes relacionados a cargas químicas, incluindo testes unitários e de integração.


