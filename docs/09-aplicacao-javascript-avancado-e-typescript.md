# Aplicação de Javascript Avançado e TypeScript
Este documento apresenta como JavaScript Avançado e TypeScript serão utilizados na implementação futura do QuimiPort.

A utilização dessas tecnologias será orientada pelos princípios de tipagem forte, separação de responsabilidades, reutilização de código e segurança durante o desenvolvimento.

## Tipagem forte
O TypeScript será utilizado para definir os tipos das nossas entidades, objetos de valor, casos de uso e demais componentes da aplicação.

A tipagem forte terá como objetivo reduzir erros durante o desenvolvimento e tornar os contratos entre as diferentes partes da aplicação mais claros.

Exemplo:

```typescript
interface CargaQuimica {
  id: string;
  produtoQuimico: ProdutoQuimico;
  classificacaoRisco: ClassificacaoRisco;
  status: StatusCarga;
  responsavelTecnico: ResponsavelTecnico;
}
```
Dessa forma, uma carga não poderá ser criada sem que os tipos esperados sejam respeitados, garantindo que a aplicação siga as regras de negócio definidas.

## Interfaces
Interfaces serão utilizadas para definir contratos entre diferentes partes da aplicação, como casos de uso, repositórios e serviços.

As interfaces permitirão que diferentes implementações possam ser utilizadas sem alterar o código que depende delas, facilitando a manutenção e evolução da aplicação.

Exemplo:
```typescript
interface CargaQuimicaRepository {
  salvar(carga: CargaQuimica): Promise<void>;
  buscarPorId(id: string): Promise<CargaQuimica | null>;
  atualizarStatus(id: string, status: StatusCarga): Promise<void>;
}
```
A implementação desse contrato poderá ser alterada sem modificar as regras de negócio do domínio.


## Classes
Classes serão utilizadas quando houver necessidade de representar entidades ou objetos que possuam estado e comportamento.

A entidade CargaQuimica poderá ser representada por uma classe, concentrando comportamentos relacionados às regras de negócio da carga.

```typescript
class CargaQuimica {
  constructor(
    public id: string,
    public produtoQuimico: ProdutoQuimico,
    public classificacaoRisco: ClassificacaoRisco,
    public status: StatusCarga,
    public responsavelTecnico: ResponsavelTecnico
  ) {}
  aprovar() {
    // Lógica para aprovar a carga, alterando o status e validando regras de negócio
  }
  reprovar() {
    // Lógica para reprovar a carga, alterando o status e validando regras de negócio
  }
}
```

## Enums
Enums serão utilizados para representar conjuntos fechados de valores do domínio.

Um exemplo é o StatusCarga:

```typescript
enum StatusCarga {
    EM_ANALISE = 'EM_ANALISE',
    EM_INSPECAO = 'EM_INSPECAO',
    APROVADA = 'APROVADA',
    REPROVADA = 'REPROVADA',
    BLOQUEADA = 'BLOQUEADA',
    LIBERADA = 'LIBERADA'
}
```

O enum será utilizado para evitar valores inválidos e manter padronizados os estados possíveis da carga.

Enums também poderão ser utilizados para classificações que possuam um conjunto definido de valores.

## Funções Puras

Funções puras serão utilizadas principalmente para validações e transformações que não dependam de estado externo.

Uma função pura sempre deverá produzir o mesmo resultado quando receber os mesmos parâmetros e não deverá modificar dados externos.

Exemplo:
```typescript
function quantidadeValida(quantidade: number): boolean {
    return quantidade > 0;
}
```
Essa abordagem facilita a criação de testes unitários e reduz efeitos colaterais no sistema.

## Módulos ES6+

O projeto utilizará o sistema de módulos do JavaScript baseado em import e export.

Exemplo:
```typescript
export class Carga {
    // ...
}
```

E sua utilização:
```typescript
import { Carga } from './Carga';
```

A utilização de módulos permitirá separar responsabilidades e evitar o acoplamento excessivo entre componentes.

## Async/Await
O uso de async/await será adotado para lidar com operações assíncronas, como chamadas a bancos de dados ou serviços externos.

Exemplo:
```typescript
async function buscarCargaPorId(id: string): Promise<CargaQuimica | null> {
    return await cargaRepository.buscarPorId(id);
}
```

## Generics
Generics serão utilizados quando houver necessidade de criar estruturas reutilizáveis que possam trabalhar com diferentes tipos.

Um possível exemplo é um resultado genérico para operações da aplicação:

```typescript
interface Result<T> {
    sucesso: boolean;
    dados?: T;
    erro?: string;
}
```

Dessa forma, o mesmo contrato poderá ser utilizado para diferentes entidades:

Result<Carga>
Result<ProdutoQuimico>
Result<Documento>

O uso de generics será aplicado somente quando trouxer benefício real para reutilização e segurança de tipos.

## Tratamento de erros
O tratamento de erros será feito utilizando exceções e mensagens claras para o usuário.

O sistema deverá possuir um padrão para tratamento de erros, evitando que falhas sejam ignoradas ou retornadas de maneira inconsistente.

Os erros deverão ser classificados de acordo com sua origem.

Exemplos:

- Erros de validação;
- Erros de regra de negócio;
- Entidade não encontrada;
- Erros de infraestrutura;
- Erros de integração.

No domínio, operações que violem regras de negócio deverão impedir a alteração do estado da entidade.

Exemplo:
```typescript
carga.aprovarCarga();
```
Caso a carga não cumpra os requisitos necessários para aprovação, a operação deverá retornar ou lançar um erro de domínio apropriado.

Para isso o uso do Try/Catch será utilizado para capturar exceções e tratar erros de forma adequada, garantindo que o sistema continue funcionando corretamente mesmo diante de falhas.

## Organização de Contratos e Tipos Compartilhados
Os contratos e tipos utilizados por diferentes partes da aplicação serão organizados de maneira centralizada quando fizer sentido.

Exemplo:

```text
src/
├── domain/
│   ├── entities/
│   ├── value-objects/
│   └── repositories/
│
├── application/
│   ├── use-cases/
│   └── contracts/
│
├── interface/
│   └── dtos/
│
└── shared/
    └── types/
```

Os tipos compartilhados deverão ser utilizados somente quando realmente representarem conceitos comuns entre diferentes módulos, evitando a criação de dependências desnecessárias.