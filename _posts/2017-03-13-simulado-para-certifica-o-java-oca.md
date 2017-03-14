---
layout: post
published: false
title: Simulado para certificação JAVA OCA
---
A intenção deste post é alertar para as armadilhas da prova (ou simulado) de certificação, através da explicação detalhada das questões que errei no primeiro simulado que realizei na semana passada, dia 06/03/2017. 

No total eram 80 questões, com duração total de 2 horas e 30 minutos, nos mesmos moldes da prova oficial, portanto. Abaixo seguem os meus números, não muito empolgadores:

- Tempo total: 2 horas e 9 minutos
- Percentual de acerto: 66% (pasmem, o mínimo é 65%)

## Questões

### Which of the following statement(s) is/are correct?

1. Java supports multiple type inheritance as well as multiple state inheritance.
2. Java supports multiple type inheritance but not multiple state inheritance.
3. Java does not support multiple type inheritance but supports multiple state inheritance.
4. Java does not support multiple type or state inheritance.

> Reposta certa é a **(2)**.
> Java permite que uma classe implemente múltiplas interfaces. Uma interface é um tipo e não contém qualquer estado, o que implica em afirmar que Java suporta herança múltipla de tipo.
> Uma classe contém estado e extender uma classe significa herdar o estado. Como Java não permite que uma classe extenda múltiplas classes, então não suporta herança múltipla e estado.

### What happens when you try to compile and run the following program?

    public class CastTest{
       public static void main(String args[ ] ){
          byte b = -128 ;
          int i = b ;
          b = (byte) i;
          System.out.println(i+" "+b);
       }
    }
    
1. The compiler will refuse to compile it because i and b are of different types cannot be assigned to each other.
2. The program will compile and will print -128 and -128 when run.
3. The compiler will refuse to compile it because -128 is outside the legal range of values for a byte.
4. The program will compile and will print 128 and -128 when run .
5. The program will compile and will print 255 and -128 when run .

> Reposta certa é **(2)**.
> Por um descuido, confundi os ranges e assinalei a opção (3).
> Os tipos byte e int permitem valores com sinal. Um byte pode ser SEMPRE atribuído para um int. O range permitido de byte é exatamente de -128 a 127.







