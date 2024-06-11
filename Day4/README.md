### VOLUMES (https://docs.docker.com/storage/volumes/)
    Nada mais é do que mapear um diretório dentro de um container. volumes nada mais são que diretórios externos ao container, que são montados diretamente nele, e dessa forma bypassam seu filesystem, ou seja, não seguem aquele padrão de camadas
- BIND - Diretório existente dentro da máquina (host) e aponta o container para esse diretório
- VOLUME - Com este item consigo fazer com que o docker crie e gerencie os volumes
    - Este volume é criado na estrutura de pastas dentro do hosts 
    - Aqui é o docker quem gerencia este diretório (diferente do tipo BIND)
    - Para este tipo de volume o diretório é criado na máquina host no path `/var/lib/docker/volumes/`
- TMPFS - Volume criado mas fica somente em memória, ou seja quando o container morre este volume morre junto

<br>

`IMPORTANTE`
- O volume é inicializado quando o container é criado;
- Caso ocorra de já haver dados no diretório em que vc está montando como volume, ou seja, se já existirem dados no diretório base utilizado para o volume (diretório na máquina host), aqueles dados serão copiados para o volume.
- Um volume pode ser reutilizado e compartilhado entre containerers.
- Alterações em um volume são feitas diretamente no volume.
- Alterações em um volume não irão com a imagem quando você fizer uma cópia ou snapshot de um container
- Volumes continuam a existir mesmo quando o container é deletado

<br>

## EXEMPLOS - BIND

- **BIND:** `docker container run -it --name teste-mount-bind --mount type=bind,source=/home/vagrant/teste-mount,target=/teste-mount debian`
- **BIND (ReadOnly)** `docker container run -it --name teste-mount-bind-ro  --mount type=bind,source=/home/vagrant/teste-mount,target=/teste-mount,ro debian`


## EXEMPLOS - VOLUMES

- **VOLUME:** 
    - `docker volume ls` <br> 
    - `docker volume create [NOME-VOLUME]` <br>
    - `docker volume inspect [NOME-VOLUME]` <br>
    - `docker volume inspect --format '{{ .Mountpoint }}' nome-volume` -> Inspect com filtro para retornar somente o path do volume

    **IMPORTANTE**
    - No caso de volumes do time `volume`, se vc criar um arquivo em um determinado volume e se o volume for excluído, então o arquivo será apagado <br>

    - **EXEMPLOS**
    - `docker run -it --name testando-volumes --mount type=volume,source=nome-volume,target=/local-container-volume debian`
        - Caso o volume não exista o volume será criado automaticamente