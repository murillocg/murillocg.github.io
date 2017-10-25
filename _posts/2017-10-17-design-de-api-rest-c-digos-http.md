---
layout: post
published: true
title: Design de API REST - Códigos HTTP
date: '2017-10-17'
---
## Introdução
O procotolo HTTP define uma série de [~60 códigos de status](https://tools.ietf.org/html/rfc7231#section-6) que podem ser retornados em API. Estes códigos são essenciais para indicar o resultado de uma requisição, ou seja, da manipulação de recursos. 

Comumente vimos APIs trabalhando apenas com o código 200, sendo necessário analisar campos específicos da resposta para entender exatamente o resultado da requisição. Esta abordagem, além de inutilizar o código de status HTTP, ainda exige um tratamento específico, sendo necessário interpretar um ou mais campos do corpo da requisição, dificultando o consumo da API por parte dos clientes.

Segue abaixo a relação dos códigos mais comuns para uso, separada por categorias, **obtida a partir da minha experiência de trabalho e estudo**:

### Sucesso

|Código          |Descrição                                                                            |
|:---------------|:------------------------------------------------------------------------------------|
|200  OK         | Resposta a um bem-sucedido GET, PUT, PATCH ou DELETE. Também pode ser usado para um POST que não resulte em uma criação. |
|201  Created    | Em requisições POST quando um recurso é criado. |
|204  No Content | Resposta a uma requisição bem-sucedida que não retorna um corpo (requisição DELETE). |

### Redirecionamento

|Código                |Descrição                                                                            |
|:---------------------|:------------------------------------------------------------------------------------|
|301 Moved Permanently | Base para criação de [encurtadores de URL](https://github.com/murillocg/tiny-url). |
|304 Not Modified      | Usado ao lidar com cabeçalhos de cache HTTP. |

### Erros do cliente da API

|Código          |Descrição                                                                            |
|:---------------|:------------------------------------------------------------------------------------|
|400 Bad Request | O pedido é malformado; não foi possível fazer o parse do corpo. |
|401 Unauthorized | Cliente não está autenticado ou os dados de autenticação são inválidos. |
|403 Forbidden | A autenticação foi bem-sucedida, mas o usuário autenticado não tem acesso ao recurso. |
|404 Not Found | Recurso solicitado não existe. |
|405 Method Not Allowed | Método HTTP que não é permitido para o usuário autenticado. |
|415 Unsupported Media Type | Se o content type incorreto foi fornecido como parte do pedido. |
|422 Unprocessable Entity | Usado para validação de erros. |

### Erros do servidor

|Código          |Descrição                                                                            |
|:---------------|:------------------------------------------------------------------------------------|
| 500 Internal Server Error | Erro genérico indicando um problema de execução no servidor (cliente pode fazer chamar novamente se faz sentido) |

## Conclusão

Existem algumas situações discutíveis em relação aos códigos de status HTTP, como por exemplo para validação de erros de uma requisição POST. É muito comum o uso do `400` neste caso, mas como visto na tabela acima, ele indica que o corpo da requisição não está dentro do esperado, pois faltou algum campo, ou há um tipo de dado diferente, e não exatamente que alguma regra de validação/domínio de campo não foi atendida. Para isso há o código `422` (baseado na [API do GitHub](https://developer.github.com/v3/#client-errors)), então a dica é sempre usar o mais específico possível conforme a situação tratada.

A intenção aqui foi fornecer um guia com recomendações e não impor uma verdade absoluta. Se curtiu o post ou tem uma consideração deixe um like ou comentário abaixo ;)

## Referências

- [GitHub REST API v3](https://developer.github.com/v3/)
- [HTTP Status Codes](https://httpstatuses.com/)
- [Melhores práticas para uma API RESTful pragmática](http://desenvolvimentoparaweb.com/miscelanea/melhores-praticas-para-uma-api-restful-pragmatica-parte-2/#http-status)
- [Zalando RESTful API Guidelines](http://zalando.github.io/restful-api-guidelines/)
- [JHipster Sample Application](https://github.com/jhipster/jhipster-sample-app-ng2)
