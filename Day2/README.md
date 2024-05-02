# Anotações - DAY 2

<br>

## Dockerfile
Dockerfile existe com o propósito/objetivo de criação de imagens de containers.

As instruções do Dockerfile vai gerando diversas camadas dentro da imagem.

- Sempre as instruções são em MAIÚSCULO;
- O nome do arquivo precisa ser `Dockerfile`

***INSTRUÇÕES***
 - `FROM`- De onde partirá a minha imagem, que é a minha imagem base
    > **IMPORTANTE**: Por questões de segurança, é SEMPRE IMPORTANTE colocar a versão da imagem que eu quero baixar, NUNCA deixar sem nada porque sem nada virá sempre a `latest` o que poderá gerar falhar na minha app;

 - `RUN` - Quando eu preciso executar/rodar algum comando quando minha imagem está sendo criada/buildada
    - Cada `RUN` significa uma nova camada na imagem

- `USER` - Determina qual usuário será utilizado na imagem. Por default é o root.

- `EXPOSE` - Indica a porta que será exposta ()

- `CMD` - Excuta um comando EM TEMPO DE EXECUÇÃO DO CONTAINER

- `COPY` - Realizar a cópia e arquivos do host (máquina que será rodado o build da imagem) para dentro da imagem;

- `WORKDIR` - Path onde será executado o "entryPoint" do container

- `ENV` - Comando para fornecimento de variáveis de ambiente

- `LABEL` - Permite passar um rótulo/metada, para auxiliar na organização da imagem

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


## MULTISTAGE
Comando para permitir enxugar o tamanho da imagem de container
- O multi-stage nada mais é do que a possibilidade de você criar uma espécie de pipeline em nosso dockerfile, podendo inclusive ter duas entradas FROM.
- Multistage é muito utilizado em cenários onde eu preciso compilar uma determinada imagem, mas não quero/preciso levar todas as libs/componentes utilizados apenas no build.
    - Por Exemplo: Em um cenário onde preciso buildar uma aplicação em **golang** eu posso realizar o build da app na primeira imagem e utilizar o MULTISTAGE para criar a segunda imagem (imagem que de fato utilizarei para criar os containers) contendo apenas os artefatos do build da app **golang** (vide exemplo 001 na seção `Multistage` no arquivo Dockerfile)


## ARG e ENV
- `ARG` - Variável válida no momento de BUILD
- `ENV` - Variável disponibilizada como `VARIÁVEL DE AMBIENTE` dentro do container

<br>

***IMPORTANTE:*** Para eu passar valor para um argumento via comando de `docker image build` eu utilizo o parâmetro `--build-arg`
- Exemplo: `docker image build -t go-custom:4.0 --build-arg GIROPOPS=girus .`

>***TRICK:*** É possível passar o valor de um ARG para uma ENV durante o processo de criação da imagem, ou seja, deste forma o valor do ARG fornecido no build estará disponível dentro da imagem/container na respectiva variárel de ambiente criada (vide EXEMPLO 009 - Dockerfile)

<br>

## VOLUME
Indica uma forma de persistir dentro do container, ou seja, caso o container seja parado os dados seguirão persistidos **no HOST** para utilização quando o container for novamente iniciado. (vide EXEMPLO 010 - Dockerfile )

>**IMPORTANTE:** Os dados ficarão persistidos no host no path indicado no `Mounts -> Source` das configuraçoes do container (configurações que podem ser acessadas via `docker container inspect`) 

<br>

## Timeline: Criação de uma Imagem Docker
Criação do Dockerfile: O primeiro passo na criação de uma imagem Docker é criar um arquivo chamado Dockerfile. Esse arquivo contém as instruções necessárias para construir a imagem, como a escolha da base, a instalação de dependências e a configuração do ambiente.

Build da Imagem: Após a criação do Dockerfile, é necessário executar o comando docker build para iniciar o processo de construção da imagem. Nesse momento, o Docker irá ler o arquivo Dockerfile e compilar a imagem conforme as instruções especificadas.

Tag da Imagem: Após a conclusão do build, é possível adicionar tags à imagem para facilitar sua identificação e versionamento. Por exemplo, podemos adicionar uma tag indicando a versão da aplicação ou o ambiente de destino.

Push da Imagem: Para disponibilizar a imagem em um repositório remoto, é necessário utilizar o comando docker push. Esse comando envia a imagem para um registro, como o Docker Hub ou um registro privado, permitindo que outras pessoas possam baixar e utilizar a imagem.

Verificação de Vulnerabilidades: É importante garantir a segurança da imagem antes de disponibilizá-la para uso. Para isso, podemos utilizar ferramentas de verificação de vulnerabilidades, como o Trivy. Essas ferramentas analisam a imagem em busca de componentes com falhas de segurança conhecidas.

Assinatura com o Cosign: Para adicionar uma camada extra de segurança, é possível assinar digitalmente a imagem utilizando o Cosign. Essa assinatura garante a integridade da imagem, permitindo que os usuários verifiquem a autenticidade e a procedência da mesma.