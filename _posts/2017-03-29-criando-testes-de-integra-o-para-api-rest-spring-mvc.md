---
layout: post
published: false
title: Criando testes de integração para API REST Spring MVC
---

Algo tão importante quanto a implementação de novos recursos é a criação de testes automatizados.
Existem diversos tipos de testes, desde unitários, aceitação, mas hoje o foco será no teste de integração.

> Colocar aqui a definição de teste de integração

Os testes foram necessários para a seguinte API REST:

    @RestController
    @RequestMapping(path = "/api/campanhas", produces = MediaType.APPLICATION_JSON_UTF8_VALUE)
    public class CampanhaResource {

        @PostMapping
        public ResponseEntity<Campanha> createCampanha(@Valid @RequestBody Campanha campanha){}

        @GetMapping
        public ResponseEntity<Page<Campanha>> getAllCampanhas(Pageable pageable) {}

        @GetMapping("/{id}")
        public ResponseEntity<Campanha> getCampanha(@PathVariable Long id) {}

        @DeleteMapping("/{id}")
        public ResponseEntity<Void> deleteCampanha(@PathVariable Long id) {}

    }
    
Além de todos os endpoints, ainda deseja-se testar as validações da API:
1. Nome da campanha obrigatório;
2. Marca da campanha obrigatório;







