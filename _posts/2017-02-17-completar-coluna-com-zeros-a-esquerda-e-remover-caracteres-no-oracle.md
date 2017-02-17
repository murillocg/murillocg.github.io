---
layout: post
published: false
title: Completar coluna com zeros a esquerda e remover caracteres no Oracle
date: '2017-02-17'
---

Recursos utilizados:

- Busca por registros que possuem caractere em campo numérico: REGEXP_LIKE(CPF, '[A-Za-z]')
- Completar campo texto com zeros à esquerda LPAD(CPF, 11, '0')
- Somente campos que possuem quantidade de caracteres diferente de x length(cpf) = 11
