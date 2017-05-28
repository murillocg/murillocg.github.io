---
layout: post
published: false
title: Introdução ao Apache Maven
---
## A New Post

### Principais comandos

`mvn compile`
Dispara o compilador associado ao projeto (no pom.xml é possível definir outras versões).
O código compilado é colocado no diretório target. Observer que o código compilado são os arquivos .class.

`mvn test`
Roda os testes do framework configurado

`mvn package`
Empacota o código compilado no formato de distribuição (jar ou war) 

`mvn install`
Este comando encapsula os comandos padrões de um ciclio de vida de build incluindo compilação, teste, empacotamento e instalação do artefato no repositório local.

### Desenvolvendo uma aplicação Web


?? O que o scopes significam ??

o scope padrão é compile.

Existem 6 diferentes escopos disponíveis, sendo eles:

* compile - Dependência necessária para compilação. Significa, por consequência, que é necessária para testes bem como em runtime (quando o projeto está rodando)

* test - Dependência necessária somente para testes. Significa que esta dependência está no código de teste. Como o código de teste não é usado para rodar o projeto, esta dependência não é necessária em runtime

* runtime - Dependência não necessária durante a compilação, mas para rodar o projeto.  It is in the runtime and test classpaths, but not the compile classpath. Um exemplo ?

* provided - This tells Maven that dependency is required for compilation and runtime,
but this dependency need not be packaged with the package for distribution.
The dependency will be provided by the user. An example of this dependency is
servlet-api. Typically, application servers have these libraries.

* system - 

* import