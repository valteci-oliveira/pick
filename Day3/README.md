# Comandos Novos

- `docker container run -e REDIS_HOST=localhost desafio-day2:1.0`
    - Com o atributo `-e` é possível criar (em tempo inicialização do container) uma variável de ambiente para o contaner 

- `docker container logs <CONTAINER_ID>`

# Performance das Imagens
- Quando estamos construindo uma imagem, é importante anlisarmos a quantidade de camadas da minha imagem e imagem base. 
O comando `history` nos ajuda com relação a isso:
    - `docker history <IMAGE_ID>`

<br>

# Vulnerabilidades das Imagens

### O que é Distroless?

"Distroless" refere-se à estratégia de construir imagens de containers que são efetivamente "sem distribuição". Isso significa que elas não contêm a distribuição típica de um sistema operacional Linux, como shell de linha de comando, utilitários ou bibliotecas desnecessárias. Ao invés disso, essas imagens contêm apenas o ambiente de runtime (como Node.js, Python, Java, etc.) e o aplicativo em si.

- Imagens que nao possuem imagem base (nao tem a instrucao FROM)
- Dentro da imagem temos somente os arquivos minimos e basicos com o que precisamos 
- Quando vc tem uma imagem com apenas uma cada vc diminui as possibilidades de vulnerabilidades

- Site para Obter imagens Distroless: `https://images.chainguard.dev/directory`

<br>

### Como identficar vulnerabilidades de imagens?

`Trivy` é uma opção de ferramenta para realizar o scan de vulnerabilidades na imagem
    - Site com detalhes para instalação: `https://aquasecurity.github.io/trivy/v0.52/getting-started/installation/`
    - Comando: `trivy image [NOME DA IMAGEM + TAG]`
        - Exemplo: `trivy image valtecioliveira/study001-sales-api:1.0`

<br>

`docker scout` é uma opção dentro do próprio docker para realizar a análise de vulnerabilidades na imagem 
    - Site com detalhes para instalação: `https://docs.docker.com/scout/install/`
    - Comando: `docker scout cves [NOME DA IMAGEM + TAG]`
        - Exemplo: `docker scout cves valtecioliveira/study001-sales-api:1.0`

<br>

> **IMPORTANTE:** O comando `docker scout recommendations [NOME IMAGEM]` exibe atualizações de imagens base disponíveis e recomendações de correção.

<br>

### Assinatura de imagens

A assinatura de imagens serve para trazer autenticidade sobre as imagens geradas.<br>
Basicamente uma assinatura garante que o conteúdo previsto pelo gerador da imagem de fato é o que existe dentro da respectiva imagem (previnindo a inclusão de pacotes maliciosos não planejados pelo criador da imagem)


`cosign` é uma sugestão de ferramenta onde podemos realizar a assinatura e validação de autenticidade de imagens

- Site: `https://github.com/sigstore/cosign`
- Instalação:
    > curl -O -L "https://github.com/sigstore/cosign/releases/latest/download/cosign-linux-amd64"
    <br>sudo mv cosign-linux-amd64 /usr/local/bin/cosign 
    <br>sudo chmod +x /usr/local/bin/cosign

<br>

- Como gerar chaves de assinatura:
     > cosign generate-key-pair
    
    >  ***Outputs:*** 
    <br>Enter password for private key: 
    <br>Enter password for private key again: 
    <br>Private key written to cosign.key
    <br>Public key written to cosign.pub

<br>

- Como assinar imagem:
    > `cosign sign -key cosign.key valtecioliveira/study001-sales-api-assinada:1.0`

    - ***IMPORTANTE:** A imagem a ser assinada precisa estar disponibilizada no Docker Hub

    - Após finalizado o processo de assinatura da imagem, será disponibilizado na página da respectiva imagem no docker hub um `sha-256` que será justamente o hash de assinatura da imagem;

- Como validar imagem assinada:
    > `cosign verify --key  cosign.pub valtecioliveira/study001-sales-api-assinada:1.0`

    > ***Outputs:***
    <br>The following checks were performed on each of these signatures:
    <br>- The cosign claims were validated
    <br>- Existence of the claims in the transparency log was verified offline
    <br>- The signatures were verified against the specified public key

    <br>

    ***IMPORTANTE***
    - Como argumento do parametro `--key` é preciso informar o path da CHAVE PÚBLICA do respectivo key-pair utilizado no processo de assinatura da imagem; 


