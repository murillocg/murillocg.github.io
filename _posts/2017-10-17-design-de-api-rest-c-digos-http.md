---
layout: post
published: false
title: Design de API REST - Códigos HTTP
date: '2017-10-17'
---
## Design de API REST - Códigos HTTP

O procotolo HTTP define uma série de [~60 códigos de status](https://tools.ietf.org/html/rfc7231#section-6) que podem ser retornados em API. Estes códigos são essenciais para indicar o resultado de uma requisição, ou seja, da manipulação de recursos. 

Comumente vimos APIs trabalhando apenas com o código 200, sendo necessário analisar campos específicos da resposta para entender exatamente o resultado da requisição. Esta abordagem, além de inutilizar o código de status HTTP, ainda exige um tratamento específico, sendo necessário interpretar um ou mais campos do corpo da requisição, dificultando o consumo da API por parte dos clientes.

Segue abaixo a relação dos códigos mais comuns para uso, separada por categorias, **obtida a partir da minha experiência de trabalho e estudo**:

### Sucesso
200 OK: Resposta a um bem-sucedido GET, PUT, PATCH ou DELETE. Também pode ser usado para um POST que não resulte em uma criação.
201 Created: Resposta a um POST que resulta em uma criação. Deve ser combinado com um Location header que aponta para a localização do novo recurso.
204 No Content: Resposta a um pedido bem-sucedido que não retornará um corpo (como uma solicitação DELETE)

### Redirecionamento
301 Moved Permanently: Base para criação de [encurtadores de URL](https://github.com/murillocg/tiny-url)
304 Not Modified: Usado ao lidar com cabeçalhos de cache HTTP

### Erros do cliente da API
400 Bad Request: O pedido é malformado; não foi possível fazer o parse do corpo
401 Unauthorized: Cliente não está autenticado ou os dados de autenticação são inválidos
403 Forbidden: A autenticação foi bem-sucedida, mas o usuário autenticado não tem acesso ao recurso
404 Not Found: Recurso solicitado não existe
405 Method Not Allowed: Método HTTP que não é permitido para o usuário autenticado está sendo solicitado.
415 Unsupported Media Type: Se o content type incorreto foi fornecido como parte do pedido.
422 Unprocessable Entity: Usado para validação de erros. 

### Erros do servidor
500 Internal Server Error: Erro genérico indicando um problema de execução no servidor (cliente pode fazer chamar novamente se faz sentido)

(consulte uma breve explicaçãod de todos os códigos em https://httpstatuses.com/)

## Conclusão

Existem algumas situações discutíveis em relação aos códigos de status HTTP, como por exemplo para validação de erros de uma requisição POST. É muito comum o uso do `400` neste caso, mas como visto na tabela acima, ele indica que o corpo da requisição não está dentro do esperado, pois faltou algum campo, ou há um tipo de dado diferente, e não exatamente que alguma regra de validação/domínio de campo não foi atendida. Para isso há o código `422`, então a sugestão é sempre usar o mais específico possível conforme a situação tratada.

A intenção aqui foi fornecer um guia com recomendações (baseadas nas minhas pequena experiências) e não impor uma verdade absoluta. ### 

Se curtiu o post, deixe um like ou comentário abaixo ;)

## Referências

- https://developer.github.com/v3/
- http://desenvolvimentoparaweb.com/miscelanea/melhores-praticas-para-uma-api-restful-pragmatica-parte-2/#http-status
- http://zalando.github.io/restful-api-guidelines/
- https://github.com/jhipster/jhipster-sample-app-ng2
