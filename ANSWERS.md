Coloque aqui suas respostas, observações e o que mais achar necessário. Mais uma vez, boa sorte!

# GetNinjas DevOps test.


## Sobre o teste
O propósito desse teste não é avaliar certo ou errado. Mas sim formas de **pensar e trabalhar** do candidato. Queremos avaliar também, **qualidade de código e simplicidade** (não confunda simplicidade com relaxo, mas sim o quão **eficiente** é o código/arquitetura proposta)
Sinta-se livre para desenvolver linhas de raciocino, comentar, etc...
E em caso de dúvidas, não tenha vergonha de enviar um email para **devops@getninjas.com.br**

## Cenário
Temos no repo https://github.com/getninjas/devops_test uma aplicação muito simples, uma API rest escrita em Golang, que atualmente só responde à rota `/healthcheck`. Essa aplicação, em compliance com o item III do 12factor app, espera alguns **parametros via ambiente** para rodar corretamente.
Outro ponto importante é que este código tem **cobertura de testes***.

## Objetivos
Dado o Cenário acima queremos que você faça o seguinte:

#### 1. Deploy da aplicação na AWS.
Pra isso, você provavelmente precisará de uma conta free-tier da AWS ou uma já existente, mas não se preocupe, não iremos olhar sua conta ou chamar a api já rodando, queremos que você crie uma forma que possamos recriar toda sua infraestrutura em nossa conta de forma simples.<br>
Mas por favor, leve em conta que seria um ambiente muito próximo à produção, portanto, **escalabilidade**, billing, segurança, **monitoramento** e **logging** tudo isso será levado em consideração na nossa análise.
Sinta-se livre para usar as ferramentas que julgue necessário, mas lembre-se: que na dúvida, vá pelo simples.
Crie um desenho que represente a arquitetura envolvida.

#### 2. Crie uma forma que possamos subir essa aplicação localmente de forma simples.
Mais uma vez, sinta-se livre para usar a ferramenta que julgar necessária.

### Kitana Deployment  

##### Ferramentas usadas

- Ansible
- Docker 
- AWS
- Kitana Application

# Tasklist

- Playbook para provisionamento das instâncias na AWS ubuntu 18.04
- Playbook de provisionamento do docker na instância AWS criada acima
    - Necessario apontamento no hosts [docker]

# Playbook de Provisionamento / Ansible

## Variáveis
- escolha da imagem 
- versionamento disso está populado no vars
- keys da aws alimentadas atraves da variavel profile
- regiao da instância escolhida
- número de instâncias

## Tasks
- task security group para dar releases nas portas necessárias
- task criação da instancias 
 - aplica security group com var
 - seta imagem através da var
 - regiao da instância através var
 - quantidade de instâncias através da var
- adiciona instância a inventário temporário
- adiciona IP no inventário ip privado
- adiciona IP no inventário ip público
- aguarda SSH
- coloca tag na instância ansible-1 (como estou usando apenas 1)

# Playbook Docker e provisionamento Kitana

## Variáveis
- número de containers docker que vou precisar
- nome da aplicação kitana-app setada
- nome da imagem atrelada ao docker hub `thiagom/kitana-app:v1`
- comando usado no docker `go run main.go`

## Tasks
- Install apt: garante que o gerenciador apt esteja lá e atualizado 
- instala pacotes necessários para o docker
- adiciona chave GPG docker
- instala repo docker
- da um apt update e instalar o docker
- instala módulos do docker do pip
- seta a nossa imagem do docker hub no docker
- cria o container
- starta o container liberando porta 8000 do kitana
- curl ocorre com sucesso


curl -i http://34.204.197.189:8000/healthcheck
HTTP/1.1 200 OK
Date: Sat, 29 Aug 2020 20:18:23 GMT
Content-Length: 24
Content-Type: text/plain; charset=utf-8

Hey Bro, Ninja is Alive!root@



# Additional Tasks
- chaves da AWS serem aplicadas no ~/aws/credentials
- chave pem para criar as instâncias com ssh-add chave.pem
- ao provisionar a instância na aws pegar o ip e alimentar o hosts do playbook docker

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

O principal objetivo de se utilizar pipeline em nossa área é o de se automatizar o processo de entrega de software o processo de entrega de software colocando-os em produção de forma rápida, ou melhor dizer de forma contínua, entretanto sem perder a qualidade da entrega.

O pipeline é, na verdade, uma sequência de etapas que precisam ser executadas para colocar a aplicação em produção, como fazer o build do código, executar testes automatizados e a implantação em ambientes de teste e produção.

Para falarmos de pipeline devemos sitar outros termos que estão todos interligados como o devops que é uma cultura de automatização de processos de infra e de desenvolvimento. Outros termos seriam a continuous integration que é um processo de automatização de build e teste de código que acontece quando um desenvolvedor faz um commit de alguma alteração no repositório do código, e também devemos citar o termo continuous delivery que consiste em entregar novos recursos aos usuários da forma mais rápida e eficiente possível.

O objetivo principal da entrega contínua é permitir o fluxo constante de atualizações disponibilizados em produção por uma espécie de linha de produção de software automatizado, o que seria um pipeline de entrega contínua.

A utilização de pipeline traz vantagens, a automatização é benéfica por si só, pois agiliza e evita os erros humanos e o número menor de erros na integração permite uma entrega mais rápida. É mais fácil integrar pequenas atualizações mesmo em grande bases de códigos. Como os desenvolvedores  ficam mais focados na construção de novas features e não precisam se preocupar em levar o seu código até a produção. Os bugs são identificados mais rapidamente fazendo com que as soluções sejam mais simples. Logs de todas as atualizações, testes e deploys são acessíveis para verificação a qualquer momento. Fazer um rollback é algo simples de ser feito.

Não existe um único modelo de pipeline a ser seguido, ele deve ser construído de acordo com as características e necessidades do projeto e das equipes envolvidas. Entretanto, podemos afirmar que um pipeline é multidisciplinar porque envolve equipes e profissionais bem distintos na criação e utilização, um bom pipeline é rápido e confiável e preciso. Segue um ciclo de vida padrão do desenvolvimento de software:


![pipeline](https://github.com/ThiagoMartinsdeMelo/devops_test/blob/master/diagrams/pipeline.png)

