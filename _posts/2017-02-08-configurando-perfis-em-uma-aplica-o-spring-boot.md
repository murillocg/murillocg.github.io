---
layout: post
published: false
title: Configurando perfis em uma aplicação Spring Boot
date: '2017-02-08'
---
O uso de perfis em aplicações é altamente recomendado por facilitar o desenvolvimento da aplicação bem como sua publicação. Através de perfis é possível ter configurações separadas por ambiente, sendo estes alguns exemplos:
- Banco de dados (tipo de banco, endereço)
- Logs (níveis de log e tipos de saída)
- Configurações de build e deploy (maven)

Neste post, será demonstrada a configuração de perfis integrados ao Maven para uma aplicação Spring Boot, o que facilita o trabalho em IDEs como o Netbeans, que permite a rápida escolha do perfil a ser utilizado.

## Criando os perfis no pom.xml

Os dois perfis **dev** e **prod** foram definidos na seção profiles, sendo o perfil **dev** ativado por padrão.

```xml
    <profiles>
        <profile>
            <id>dev</id>
            <activation>
                <activeByDefault>true</activeByDefault>
            </activation>
            <properties>
                <!-- log configuration -->
                <logback.loglevel>DEBUG</logback.loglevel>
                <!-- default Spring profiles -->
                <spring.profiles.active>dev</spring.profiles.active>
            </properties>
            <dependencies>
                <dependency>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-devtools</artifactId>
                    <optional>true</optional>
                </dependency>
            </dependencies>
        </profile>
        <profile>
            <id>prod</id>
            <properties>
                <!-- log configuration -->
                <logback.loglevel>INFO</logback.loglevel>
                <!-- default Spring profiles -->
                <spring.profiles.active>prod</spring.profiles.active>
            </properties>
        </profile>
    </profiles>
```    

## Arquivos de perfis

### application.yml

```yml
spring:
    application:
        name: horacerta
    profiles:
        # The commented value for `active` can be replaced with valid Spring profiles to load.
        # Otherwise, it will be filled in by maven when building the WAR file
        # Either way, it can be overridden by `--spring.profiles.active` value passed in the commandline or `-Dspring.profiles.active` set in `JAVA_OPTS`
        active: #spring.profiles.active#
    jackson:
        serialization.write_dates_as_timestamps: false
```

### application-dev.yml

```yml
spring:
    profiles:
        active: dev
        include: swagger
    devtools:
        restart:
            enabled: true
        livereload:
            enabled: false # we use gulp + BrowserSync for livereload
    jackson:
        serialization.indent_output: true
    datasource:
        type: com.zaxxer.hikari.HikariDataSource
        url: jdbc:h2:file:./target/h2db/db/horacerta;DB_CLOSE_DELAY=-1
        name:
        username: horacerta
        password:
```

### application-prod.yml

```yml
spring:
    devtools:
        restart:
            enabled: false
        livereload:
            enabled: false
    datasource:
        type: com.zaxxer.hikari.HikariDataSource
        url: jdbc:postgresql://localhost:5432/horacerta
        name:
        username: horacerta
        password:
```


