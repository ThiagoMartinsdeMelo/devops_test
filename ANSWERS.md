Coloque aqui suas respostas, observações e o que mais achar necessário. Mais uma vez, boa sorte!

#### 3. Coloque esta aplicação em um fluxo de CI que realize teste neste código

Segue o link com o build do teste:

https://travis-ci.org/github/ThiagoMartinsdeMelo/devops_test


------------

#### 4. Altere o nome da aplicação.
Fazendo um `curl -i http://endpoint-da-aplicacao/healthcheck` a resposta se assemelha a isto:
``` 
HTTP/1.1 200 OK
Date: Wed, 05 Sep 2018 18:07:41 GMT
Content-Length: 24
Content-Type: text/plain; charset=utf-8

Hey Bro, Ninja is Alive!%
```

Como podemos alterar o "nome da aplicação (Ninja)", para outro nome? Nos envie a saída do seu `curl` com a resposta, já com o nome alterado.

Podemos alterar o nome da aplicação editando a linha 19 do arquivo main.go e assim atribuindo um novo valor a variável de ambiente APP_NAME:

```go
appname := getEnv("APP_NAME", "Pearl Jam")
```
```bash
    $ curl -i http://localhost:8000/healthcheck   HTTP/1.1 200 OK
    Date: Fri, 28 Aug 2020 18:08:37 GMT
    Content-Length: 25
    Content-Type: text/plain; charset=utf-8
    
    Hey Bro, Pearl Jam is Alive!
```

------------

#### 5. Discorra qual (ou quais) processos você adotaria para garantir uma entrega contínua desta aplicação, desde o desenvolvimento, até a produção.





