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
