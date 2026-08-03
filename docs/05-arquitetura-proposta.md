# Arquitetura Proposta

## Objetivo

O QuimiPort será desenvolvido seguindo uma arquitetura em camadas, separando as responsabilidades da aplicação para facilitar manutenção, testes e evolução futura.

O nosso grupo inicialmente está dividido por domínios, sendo eles: Cargas Químicas (Ygor), Documentações de cargas e produtos(Letícia), Inspeção (Giovana), Produtos (Paulo), Usuários(Matheus).

## Arquitetura escolhida
A arquitetura escolhida para o desenvolvimento foi o Clean Architecture, um dos motivos foi por ser a mais recente, a popularidade, a facilidade de manutenção, experiências prévias com essa arquitetura e a separação de responsabilidades.

## Camadas da Arquitetura
A arquitetura em camadas do QuimiPort será composta pelas seguintes camadas:

```Mermaid
flowchart TB
    A["Frontend"] --> B["Interface"]
    B --> C["Application"] & n1["Test"]
    C --> D["Domain"]
    D --> E["Infrastructure"]
    E --> F[("Database")]
```

### Apresentação (Interface):
Será a camada responsável pela interação com o usuário contendo os endpoints da API e controladores.

### Aplicação (Application):
Responsável pelos casos de uso da aplicação, nele conterá os services, por exemplo: Registar usuário, Aprovar carga, etc. Essa camada será responsável por redirecionar a requisição para a camada de domínio responsável pela regra de negócio.

### Teste (Test):
Camada responsável pelos testes unitários e de integração da aplicação, garantindo que as funcionalidades estejam funcionando corretamente, que as regras de negócio estejam sendo respeitadas e auxiliando na manutenção e documentação da aplicação.

### Domínio (Domain):
Camada responsável pela regra de negócio da aplicação, tendo as Entidades, Objeto de valor, Agregados e as funções para manipulação das entidades.

### Infraestrutura (Infrastructure):
Camada responsável pela comunicação com os recursos externos, dentre eles o banco de dados, armazenamento de arquivos e serviços de terceiros.


