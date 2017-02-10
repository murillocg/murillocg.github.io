---
layout: post
published: false
title: Atualizar coluna no Oracle com data de última alteração do registro
---

## Criando uma tabela para teste

```sql
CREATE TABLE PERSON (
   ID          NUMBER(5) PRIMARY KEY,
   NAME        VARCHAR2(30),
   BIRTHDAY    DATE,
   LAST_UPDATE DATE
)
```

Após a criação da tabela, é necessário inserir alguns registros, conforme segue:

```sql
INSERT INTO PERSON VALUES (1, 'PESSOA 1', TO_DATE('01/01/1991','DD/MM/YYYY'), NULL);
INSERT INTO PERSON VALUES (2, 'PESSOA 2', TO_DATE('02/02/1992','DD/MM/YYYY'), NULL);
INSERT INTO PERSON VALUES (3, 'PESSOA 3', TO_DATE('03/03/1993','DD/MM/YYYY'), NULL);
COMMIT;
```
## Trigger para atualização da data

Agora já é possível criar e testar a trigger que será encarregada de atualizar a coluna da tabela toda vez que ocorrer um **INSERT** ou **UPDATE** na mesma. Utilizando este recurso vinculado à tabela do banco de dados, garante-se que a coluna será atualizada independente da origem da alteração da tabela: sistema principal, integração de terceiros, alteração manual, etc. Esta trigger poderia ainda ser evoluída para atualizar outra coluna com o usuário logado no banco de dados. 

```sql
CREATE OR REPLACE TRIGGER PERSON_LAST_UPDATE
   BEFORE INSERT OR UPDATE ON PERSON
   REFERENCING OLD AS old_row NEW AS new_row
   FOR EACH ROW
BEGIN
   IF    :new_row.LAST_UPDATE <> SYSDATE
      OR :new_row.LAST_UPDATE IS NULL
   THEN
      SELECT SYSDATE
        INTO :new_row.LAST_UPDATE
        FROM DUAL;
   END IF;
END;
```
## Testes

Agora é possível testar inserindo um registro. Note que o insert possui NULL no campo de LAST_UPDATE, justamente para deixar explícito que não está sendo populado por este comando.

```sql
INSERT INTO PERSON VALUES (4, 'PESSOA 4', TO_DATE('04/04/1994','DD/MM/YYYY'), NULL);
```

Realizando a consulta, pode-se perceber que a coluna foi devidamente carregada com a data+hora atual:

```sql
select NAME, BIRTHDAY, TO_CHAR(LAST_UPDATE, 'DD/MM/YYYY HH:MI:SS') FROM PERSON WHERE ID = 4
```

![consulta_update.png](/img/consulta_update.png)

## Referências:

- http://stackoverflow.com/a/1614291/5230740

