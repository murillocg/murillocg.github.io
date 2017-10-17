---
layout: post
published: false
title: Design de API REST - Códigos HTTP
date: '2017-10-17'
---
## Design de API REST - Códigos HTTP

O procotolo HTTP define uma série de [~60 códigos de status](https://tools.ietf.org/html/rfc7231#section-6) que podem ser retornados em API. Estes códigos são essenciais para indicar o resultado de uma requisição, ou seja, da manipulação de recursos. 

Comumente vimos APIs trabalhando apenas com o código 200, sendo necessário analisar campos específicos da resposta para entender exatamente o resultado da requisição. Esta abordagem, além de inutilizar o código de status HTTP, ainda exige um tratamento específico, sendo necessário interpretar um ou mais campos do corpo da requisição, dificultando o consumo da API por parte dos clientes.

Segue abaixo a relação dos códigos mais comuns para uso, separada por categorias, **obtida a partir da minha experiência tanto no trabalho como em estudo**:

### Sucesso
200 OK: Resposta a um bem-sucedido GET, PUT, PATCH ou DELETE. Também pode ser usado para um POST que não resulte em uma criação.
201 Created: Resposta a um POST que resulta em uma criação. Deve ser combinado com um Location header que aponta para a localização do novo recurso.
204 No Content: Resposta a um pedido bem-sucedido que não retornará um corpo (como uma solicitação DELETE)

### Redirecionamento
301 Moved Permanently: Base para criação de [encurtadores de URL](https://github.com/murillocg/tiny-url)
304 Not Modified: Usado quando cabeçalhos de cache HTTP estão em jogo

### Erros do cliente da API
400 Bad Request: O pedido é malformado; não foi possível fazer o parse do corpo
401 Unauthorized: Quando não são fornecidos detalhes de autenticação ou estes são inválidos.
403 Forbidden: Quando a autenticação foi bem-sucedida, mas o usuário autenticado não tem acesso ao recurso.
404 Not Found: Quando um recurso inexistente é solicitado.
405 Method Not Allowed: Quando um método HTTP que não é permitido para o usuário autenticado está sendo solicitado.
415 Unsupported Media Type: Se o content type incorreto foi fornecido como parte do pedido.
422 Unprocessable Entity: Usado para validação de erros.

### Erros do servidor
500 Internal Server Error - a generic error indication for an unexpected server execution problem (here, client retry may be senseful)

(consulte uma breve explicaçãod de todos os códigos em https://httpstatuses.com/)

## Conclusão



Se curtiu o post, deixe o comentário abaixo ;)

## Referências

- http://desenvolvimentoparaweb.com/miscelanea/melhores-praticas-para-uma-api-restful-pragmatica-parte-2/#http-status
- http://zalando.github.io/restful-api-guidelines/