# Anotações - DAY 2

<br>

## Dockerfile
Dockerfile existe com o propósito/objetivo de criação de imagens de containers.

As instruções do Dockerfile vai gerando diversas camadas dentro da imagem.

- Sempre as instruções são em MAIÚSCULO;
- O nome do arquivo precisa ser `Dockerfile`

***INSTRUÇÕES***
 - `FROM`- De onde partirá a minha imagem, que é a minha imagem base
 - `RUN` - Quando eu preciso executar/rodar algum comando quando minha imagem está sendo criada/buildada
    - Cada `RUN` significa uma nova camada na imagem

- `USER` - Determina qual usuário será utilizado na imagem. Por default é o root.

- `EXPOSE` - Indica a porta que será exposta ()

- `CMD` - Excuta um comando EM TEMPO DE EXECUÇÃO DO CONTAINER

- `COPY` - Realizar a cópia e arquivos do host (máquina que será rodado o build da imagem) para dentro da imagem;

- `WORKDIR` - Path onde será executado o "entryPoint" do container

- `ENV` - Comando para fornecimento de variáveis de ambiente

- `ENTRYPOINT` - Indica qual é o ***principal processo***, ou seja, o processo que se ele morrer o container morrerá tb
    - Quando eu tenho um `ENTRYPOINT` setado, o `CMD` será sempre relativo ao que está sendo executado no entryPoint
    - Por exemplo, se eu tiver um **nginx** no entryPoint, meus comandos no CMD poderão ser somente comandos relacionados ao Nginx

- `ADD` - É semelhante ao `COPY`, mas a grande diferença é que ele consegue fazer coisas a mais como **baixar da internet, descompactar arquivos compactados, etc**
    - Caso não seja necessário executar alguma destas ações "diferenciadas", podemos seguir usando o `COPY` mesmo
    - **IMPORTANTE:** O `ADD` apenas descompacta arquivos que forem obtidos da máquina local, ou seja, não descompacta arquivos baixados da internet.


- `HEALTHCHECK` - Comando utilizado para incluir rotina de validação da saúde do container
    - `timeout=2s` - Indica o tempo máximo de espera da resposta
    - `CMD` - Instrução para indicar o respectivo comando que será executado para validar a saúde do container 
        - Por exemplo: `curl localhost || exit 1`
            - `||` - Bloco **OU** (indicando que caso o primeiro bloco seja FALSE o será executado)
            - `exit 1` - Instrução para encerrar a execução (com ERRO), ou seja, matar o container


- `VOLUME` - Define o volume onde será montado o container



## Build Imagem
    
- `docker image build -t meu-nginx:3.0 .`
    - `-t` - Para para poder tagear a imagem (com **meu-nginx** no caso)
    - `:3.0` - Para indicar a versão da imagem
    - `.` - Indica para o build que a localização `Dockerfile`, no caso do `.` indica que o arquivo está no mesmo diretório onde está rodando o `docker build`

