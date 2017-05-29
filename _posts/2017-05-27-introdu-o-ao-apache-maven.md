---
layout: post
published: false
title: Introdução ao Apache Maven
---
Em todos os projetos Java que trabalhei até agora, sempre utilizei o Maven como ferramenta para gerenciar dependências e fazer build do projeto. São esses dois aspectos da construção do software que o Maven se propõe a resolver e na minha opinião, o faz muito bem.

O Maven é uma ferramenta relativamente fácil de usar na maioria dos projetos, havendo uma vasta quantidade de [projetos opensource](https://spring.io/guides) utilizando-o e até [ferramentas para geração de projetos](https://start.spring.io/). Ainda assim, no dia-a-dia do trabalho e estudos surgem algumas abordagens de uso diferentes das tradicionais, o que gera dúvidas intrigantes. Pois bem, o objetivo deste post é responder estas questões, não por dedução como às vezes acontece, mas por pesquisa.

1. O que são os scopes existentes nas dependências?
2. Qual a diferença entre mvn package e mvn install
3. Como ver todas as dependências, inclusive as implícitas?
4. O que são os termos goal e execution, eventualmente presente em plugins?

### Ciclo de vida, fases e objetivos

Um conceito muito importante por trás do Maven é o ciclo de vida do projeto. Um ciclo de vida possui estágios, chamados de fases. Em cada fase, um ou mais objetivos podem ser executados.

O Maven possui nativamente 3 ciclos de vida: clean, site, default. Ao utilizar a ferramenta, estes ciclos não são mencionados, mas sim a fase. A partir da fase, o Maven infere o ciclo de vida. `mvn package`, por exemplo indica o ciclo de vida `default` (visto em seguida), então este ciclo é executado. As fases são executadas em sequência, até a fase especificada no comando.

Já que cada ciclo de vida possui um número de fases, é importante listar as fases importantes para cada ciclo:

- Ciclo _clean_: A fase clean remove todos arquivos e diretórios criados pelo Maven como parte do build.

- Ciclo _site_: A fase site gera a documentação do projeto, que pode ser publicada ou customizada.

- Ciclo _default_: Compreende um conjunto de fases para lidar com o build e deploy do projeto, sendo elas:
1. _validate_: Valida se todas informações do projeto estão disponíveis e corretas;
2. _compile_: Compila o código fonte;
3. _test_: Roda os testes unitários dentro do framework configurado;
4. _package_: Empacota o código compilado no formato de distribuição (jar ou war);
5. _verify_: Faz checagens para verificar se o pacote é válido;
6. _install_: Instala o pacote no repositório local para uso como uma dependência em outros projetos localmente;
7. _deploy_: Instala o pacote final no repositório configurado;

Cada fase é composta por plugins de objetivos. Um plugin é uma tarefa específica que faz o build do projeto. Segue abaixo a tabela de algumas fases e seus respectivos plugins e objetivos:

| Fase    | Plugin                | Objetivo  |
|---------|-----------------------|-----------|
| clean   | Maven Clean plugin    | clean     |
| site    | Maven Site plugin     | site      |
| compile | Maven Compiler plugin | compile   |
| test    | Maven Surefire plugin | test      |
| package | Depende do packaging  | jar       |
| install | Maven Install plugin  | install   |

### Entendendo o pom.xml

Todo projeto Maven possui um arquivo pom que define tudo sobre o projeto e como ele é construído. Pom é um acrônomo para **project object model**. Segue abaixo os principais elementos presentes no pom.xml:

<groupId>...</groupId> -> Identificador da organização à qual o projeto pertence. É uma boa prática seguir a notação de domínio invertido para especificá-lo, como por exemplo, `io.github.murillocg`. 

<artifactId>...</artifactId> -> É o nome do projeto. Um exemplo válido é `horacerta`.

<version>...</version> -> É a versão do código do projeto. Enquanto o projeto está em desenvolvimento é comum levar o prefixo `1.0.SNAPSHOT`.

<packaging>...</packaging> -> Indica o tipo de artefato do projeto, sendo `jar` e `war` os mais comuns. 

### Comandos comuns

`mvn compile`
Dispara o compilador associado ao projeto (no pom.xml é possível definir outras versões).
O código compilado é colocado no diretório target. Observer que o código compilado são os arquivos .class.

`mvn test`
Roda os testes do framework configurado

`mvn package`
Empacota o código compilado no formato de distribuição (jar ou war) 

`mvn install`
Comando utilizado para construir e instalar o artefato no repositório local.
Este comando executa cada fase do ciclio de vida `default` em ordem (`validate`, `compile`, `package`, etc.), antes de executar `install`. 

### Escopo das dependências

o scope padrão é compile.

Existem 6 diferentes escopos disponíveis, sendo eles:

* compile - Dependência necessária para compilação. Significa, por consequência, que é necessária para testes bem como em runtime (quando o projeto está rodando)

* test - Dependência necessária somente para testes. Significa que esta dependência está no código de teste. Como o código de teste não é usado para rodar o projeto, esta dependência não é necessária em runtime

* runtime - Dependência não necessária durante a compilação, mas para rodar o projeto.  It is in the runtime and test classpaths, but not the compile classpath. Um exemplo ?

* provided - Dependência necessária para compilação e runtime, mas não empacotada com o projeto para distribuição. Esta dependência será fornecida pelo usuário. Um exemplo desta dependência é `servlet-api`, em geral fornecida pelos servidores de aplicação.

* system - Similar a `provided`, porém necessita ser indicado explicitamente a localização do arquivo JAR. Pouco utilizada. 

* import - Dependência utilizada para centralizar projetos com vários módulos. 

### Visualizar dependências

É muito útil visualizar as dependências do projeto para resolver problemas ou mesmo para manter esta configuração o mais simples possível. O plugin dependências do Maven possui uma série de objetivos (goals) para obter informações detalhadas, então vamos ver as mais úteis. 

`mvn dependency:tree`
Exibe as dependências no formato de árvore, muito útil em projetos recentes, que utilizam Spring Boot, com seu modelo de dependências agrupadas (spring-boot-starter-data-jpa, spring-boot-starter-security, spring-boot-starter-web).

```
[INFO] Scanning for projects...
[INFO]
[INFO] ------------------------------------------------------------------------
[INFO] Building duolingo 0.0.1-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO]
[INFO] --- maven-dependency-plugin:2.10:tree (default-cli) @ duolingo ---
[INFO] com.agilelabs:duolingo:jar:0.0.1-SNAPSHOT
[INFO] +- org.springframework.boot:spring-boot-starter-actuator:jar:1.5.3.RELEASE:compile
[INFO] |  +- org.springframework.boot:spring-boot-starter:jar:1.5.3.RELEASE:compile
[INFO] |  |  \- org.springframework.boot:spring-boot-starter-logging:jar:1.5.3.RELEASE:compile
[INFO] |  |     +- ch.qos.logback:logback-classic:jar:1.1.11:compile
[INFO] |  |     |  \- ch.qos.logback:logback-core:jar:1.1.11:compile
[INFO] |  |     +- org.slf4j:jul-to-slf4j:jar:1.7.25:compile
[INFO] |  |     \- org.slf4j:log4j-over-slf4j:jar:1.7.25:compile
[INFO] |  \- org.springframework.boot:spring-boot-actuator:jar:1.5.3.RELEASE:compile
[INFO] |     \- org.springframework:spring-context:jar:4.3.8.RELEASE:compile
[INFO] +- org.springframework.boot:spring-boot-starter-data-jpa:jar:1.5.3.RELEASE:compile
[INFO] |  +- org.springframework.boot:spring-boot-starter-aop:jar:1.5.3.RELEASE:compile
[INFO] |  |  \- org.aspectj:aspectjweaver:jar:1.8.10:compile
[INFO] |  +- org.springframework.boot:spring-boot-starter-jdbc:jar:1.5.3.RELEASE:compile
[INFO] |  |  +- org.apache.tomcat:tomcat-jdbc:jar:8.5.14:compile
[INFO] |  |  |  \- org.apache.tomcat:tomcat-juli:jar:8.5.14:compile
[INFO] |  |  \- org.springframework:spring-jdbc:jar:4.3.8.RELEASE:compile
[INFO] |  +- org.hibernate:hibernate-core:jar:5.0.12.Final:compile
[INFO] |  |  +- org.jboss.logging:jboss-logging:jar:3.3.1.Final:compile
[INFO] |  |  +- org.hibernate.javax.persistence:hibernate-jpa-2.1-api:jar:1.0.0.Final:compile
[INFO] |  |  +- org.javassist:javassist:jar:3.21.0-GA:compile
[INFO] |  |  +- antlr:antlr:jar:2.7.7:compile
[INFO] |  |  +- org.jboss:jandex:jar:2.0.0.Final:compile
[INFO] |  |  +- dom4j:dom4j:jar:1.6.1:compile
[INFO] |  |  \- org.hibernate.common:hibernate-commons-annotations:jar:5.0.1.Final:compile
````

`mvn dependency:analyze`
Exibe as dependências não usadas e não declaradas.

### Desenvolvimento Java com Maven

mvn clean package

mvn –DskipTests


