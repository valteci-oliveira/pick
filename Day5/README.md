# NETWORKS

É uma forma de segmentar a conectividade entre containers

- `Overlay` - Driver que permite a comunicação entre container em diversos clusters (era muito útil para a utilização com Swarm)

<br>

`docker network create` <br>
`docker network connect` <br>
`docker network inspect [NOME DA REDE]`<br>

- A partir do momento que os container estão na mesma rede, consegue ser acessados através do nome do container, não sendo mais necessário trabalhar com IPs

<br>

### COMANDO PARA CONECTAR UM CONTAINER A UM REDE

- `OPÇÃO 01:` Informando a rede diretamente no comando `run`
    - `docker container run -d --name giropops-senhas --network nome-minha-rede --env REDIS=HOST=redis -p 5000:5000 valtecioliveira/giropops-senhas:1.0`

<br>

- `OPÇÃO 02:` Conectando o respectivo container à network
    - `docker network connect [NOME DA REDE] [NOME CONTAINER]`



