# Comandos - DAY 1

`unshare --mount --uts --ipc -- net --pid --user --fork --map-root-user chroot /debian bash`

- Este comando serve para montar um ambiente com isolamento via *namespaces*, onde todos os componentes passados como parametros subirão com isolamento da máquina host;

- Este comando basicamente abrirá um bash apontando para a pasta `/debian` (onde baixamos uma cópia do debian via **debootstrap**)*

<br>

`mount -t proc none /proc`
- Uma vez dentro rodando o comando acima e já dentro do namespace, esse comando montará um *filesystem* do tipo **proc** apontando para o path informado (no caso o `/proc`contido dentro da pasta `/debian`) 

<br>

`mount -t sysfs none /sys`  
`mount -t tmpfs none /tmp`
- Faz a montagem dos *filesystems* igual ao executado acima no `/proc`

<br>

`lsns` 
- comando responsável para listar as namespaces criadas no ambiente (deve ser executado fora do isolamento criado via `unshare`)

<br>

## cgroups

`apt-get install cgroup-tools`
- Ferramenta para manipulação/interação com o `cgroup`, exemplo `cgcreate` (para criação de um novo grupo)

- `cgcreate -g cpu, memory, blkio, devices, freezer :giropops`
    - Comando para criar grupo para os controllers indicados (cpu, memory, etc), como o nome do grupo de `giropops`
    - Uma vez este comando funcionando, será criada uma pasta ***giropops*** dentro da estrutura `/sys/fs/cgroup/[cpu|memory|blkio|devices|freezer]/` 

    > **TODO**: Retornar neste ponto nos testes locais, pois ainda não está funcionando (provavelmente devido à versão do Linux no VBox)

<br>

- `cat /sys/fs/cgroup/[cpu|memory|blkio|devices|freezer]/giropops`
    - Comando para listar/validar o conteúdo da pasta `giropops` para os respectivos tipos (cpu, memory, etc..)

<br>


- `cgclassify -g cpu, memory, blkio, devices, freezer :giropops 1234`
    - Comando para realizar o vinculo do respectivo processo (pid) que está rodando o namespace criado (via unshare), com o respectivo grupo criado (via cgcreate);
    - Como já indicado mais acima, para encontrar o pid do namespace criado, basta rodar o comando `lsns` (rodar na máquina host, ou seja fora do bash do `unshare`)

<br>

- `cat /sys/fs/cgroup/cpu/giropops/cpu.cfs_period_us`
    - O arquivo `cpu.cfs_period_us` possui o valor de um ciclo completo de cpu da máquina (host);

<br>

- `cgset -r cpu.cfs_quota_us=1000 giropops`
    - Comando para setar um respectivo valor de limite de **cpu** para o grupo
    - Caso o arquivo `cpu.cfs_period_us` tivesse uma valor (tempo de ciclo de cpu) de `100000`, com o comando acima eu estaria criando um limite de cpu de `1%` para este grupo

<br>

- `cgset -r memory.limit_in_bytes=48M giropops`
    - Comando para setar um respectivo valor de limite de **memory** para o grupo
    - OBS: O valor pode ser setado em Megas (M) que a aplicação entende/converte para bytes

<br>

- `htop` 
    - Comando para listar o processamento de cpu da máquina

<br>

- `dd if=/dev/zero of catota.img bs=8k count=256k`
    - Comando para realizar a criação de arquivo com blocos de bytes, afim de gerar alto processamento na máquina

<br>

- `cgget -r memory.limit_in_bytes giropops`
    - Comando para recuperar o valor de um respectivo arquivo (semelhante à execução de um `cat` no arquivo)
   


## COMANDOS - Docker

<br>

- `curl -fsSL https://get.docker.com/ | bash`
    - Comando para realizar a instalação do Docker

<br>

- `docker container ls -a`
    - Lista os containers ativos e já finalizados

<br>

- `docker container run -it ubuntu`
    - Comando para rodar um container em modo "interativo" (dentro do container)
        - `-t` Disponibiliza um TTY (console) para o nosso container.
        - `-i` Mantém o STDIN aberto mesmo que você não esteja conectado no container.


    - `ctrl + d`: Faz com que eu saia do container, matando o container (isso por que o bash morreu)
    - `ctrl + p + q`: Faz com que eu saia do container, sem matar o container 

<br>

- `docker container attach ID_CONTAINER`
    - Permite que entre em um container EM EXECUÇÃO
    - Caso eu tivesse dado um `docker run` eu não entraria em um container, mas sim criaria um container novo

- `docker container pause ID_CONTAINER|CONTAINER_NAME`
    - Pausa um container em execução

- `docker contaienr unpause ID_CONTAINER|CONTAINER_NAME`
    - Retorna um container pausado

- `docker container stop ID_CONTAINER|CONTAINER_NAME`
    - Pausa respectivo container

- `docker container start ID_CONTAINER|CONTAINER_NAME`
    - Inicia um respectivo container parado

- `docker container rm ID_CONTAINER|CONTAINER_NAME`
    - Deleta um resoectivo container
    - Para forçar a remoção de um container em execução, será preciso usar o `-f` para forçar a remoção
        - `docker container rm -f ID_CONTAINER|CONTAINER_NAME`

- `docker container stats`
    - Mostra as estatísticas dos container que estão em execução

- `docker container top ID_CONTAINER`
    - Vai mostrar todos os processos que estão em execução em determinado container

- `docker container logs ID_CONTAINER`
    - Lista todos os logs do respectivo container

- `docker container logs -f ID_CONTAINER`
    - Lista todos os logs (como comando acima), mas vai "appendando" novas linhas no final conforme forem surgindo novos logs
    - Para sair dos logs `ctrl+c`

- `docker container prune`
    - Remove todos os containers PARADOS


- `docker image ls | docker images`
    - Lista todas as imagens salvas na máquina

- `docker image rm IMAGE_ID`
    - Remove respectiva imagem
    - **IMPORTANTE:** Caso a imagem esteja vinculada a algum container criado, será preciso excluir o container antes de remover a imagem


<br>

## Inspect
Utilizado para realizar a inspeção dos detalher de determinado recurso.
    Na versão anterior do CLI era utilizado via `docker inspect ID_RECURSO`, e foi ajustado para ser via contexto conforme exemplos abaixo:

- `docker container inspect ID_CONTAINER|CONTAINER_NAME`
- `docker image inspect IMAGE_ID `

<br>

## Modo Deamon

- `docker container run -d --name meu-nginx nginx`
    - Comando para criar/rodar um container em modo `Daemon`(segundo plano), ou seja sem a necessidade deu eu precisar ficar plugado no container para ele se manter de pé

- `docker container exec -it ID_CONTAINER|CONTAINER_NAME ls /`
    - O comando `exec` serve para eu executar algo DENTRO do respectivo container


- `docker container run -d -p 8080:80 --name CONTAINER_NAME nginx`
    - Comando para criar um container do *nginx* em modo *Deamon*, apontando para a porta 8080 do host


- `docker pull ubuntu`
    - Comando para APENAS baixar uma imagem, e deixar salvo

- `docker container create ---name CONTAINER_NAME nginx`
    - Comando para APENAS criar um container, sem realizar a execução dele

<br>