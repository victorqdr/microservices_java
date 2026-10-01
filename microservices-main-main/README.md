# Microservices - product-api e currency-api

Projeto acadêmico da disciplina de Microsserviços - Atitus Educação.

O repositório contém dois microsserviços Spring Boot que buscam suas configurações
no HashiCorp Consul (Externalized Configuration) e utilizam PostgreSQL com Flyway.

## Estrutura

```text
microservices-main/
├── product-api/          Microsserviço de produtos (porta 8000)
├── currency-api/         Microsserviço de cotações (porta 8100)
├── start_consul.bat      Script de inicialização do Consul
└── .gitignore
```

## Tecnologias

- Java 21
- Spring Boot
- Spring Data JPA
- Spring Cloud Consul Config
- PostgreSQL
- Flyway
- Maven

## Pré-requisitos

- JDK 21
- PostgreSQL instalado e em execução
- Consul disponível no PATH do sistema
- Bancos criados no PostgreSQL: `db_product` e `db_currency`

## Como executar

1. Executar o script `start_consul.bat` (inicia o Consul e cadastra as configurações).
   Manter a janela aberta.
2. Executar a classe `ProductApiApplication` (sobe na porta 8000).
3. Executar a classe `CurrencyApiApplication` (sobe na porta 8100).

As tabelas e os dados iniciais são criados automaticamente pelo Flyway na
primeira execução de cada aplicação.

## Endpoints

### product-api

```text
GET http://localhost:8000/products?targetCurrency=BRL
```

### currency-api

```text
GET http://localhost:8100/currencies?source=USD&target=BRL
```

Exemplo de resposta:

```json
{
  "sourceCurrency": "USD",
  "targetCurrency": "BRL",
  "conversionRate": 5.15,
  "environment": "Currency API running in Port: 8100"
}
```

Quando a cotação não é encontrada, a API retorna status 404 com a mensagem
"Cotação não encontrada".

## Configuração

As propriedades de porta e de conexão com o banco não ficam nos arquivos locais
das aplicações. Elas são cadastradas no Consul pelo `start_consul.bat` e
carregadas na inicialização de cada microsserviço.

Chaves utilizadas:

```text
config/product-api/
config/currency-api/
```

## Estrutura do currency-api

```text
br.edu.atitus.currencyapi
├── controllers     CurrencyController
├── dtos            CurrencyResponse
├── entities        CurrencyEntity
├── repositories    CurrencyRepository
├── services        CurrencyService
└── servicesimpl    CurrencyServiceJpa
```

## Observação

O Consul é executado em modo `-dev`, mantendo os dados apenas em memória.
Ao reiniciar o Consul é necessário executar novamente o `start_consul.bat`
para recarregar as configurações.
