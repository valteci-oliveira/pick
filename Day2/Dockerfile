# ##############
# EXEMPLO 001
# ##############
# 1. Inclusão da instrução FROM
# 2. Inclusão da instrução RUN
# 3. Inclusão da instrução EXPOSE
# 4. Inclusāo da instrução CMD 
# 4.1 Neste exemplo o "-g" indica que o nginx será executado em primeiro plano
FROM ubuntu:18.04
RUN apt-get update && apt-get install nginx -y
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]


# ##############
# EXEMPLO 002
# ##############
# 1. Inclusão na instrução RUN do comando `rm -rf /var/lib/apt/lists/*`
# 1.1. Este comando serve para remover os arquivos "lixo" deixados pela instalação
# 1.2. Este comando precisa ser executado na mesma instrução RUN da instalação pois estará na mesma camada da imagem (se fosse em um RUN apartado não daria certo pois os arquivos a ser excluídos já estariam como ReadOnly)
# 2. Inclusão da instrução COPY
# 3. Inclusão da instrução WORKDIR
# 4. Inclusão da instrução ENV
FROM ubuntu:18.04
RUN apt-get update && apt-get install nginx -y && rm -rf /var/lib/apt/lists/*
EXPOSE 80
COPY index.html /var/www/html/
CMD ["nginx", "-g", "daemon off;"]
WORKDIR /var/www/html
ENV APP_VERSION 1.0.0


# ##############
# EXEMPLO 003
# ##############
# 1. Inclusão da instrução LABEL
# 2. Inclusão da instrução ENTRYPOINT
FROM ubuntu:18.04
LABEL maintainer="jeferson@linuxtips.io"
RUN apt-get update && apt-get install nginx -y && rm -rf /var/lib/apt/lists/*
EXPOSE 80
COPY index.html /var/www/html/
WORKDIR /var/www/html
ENV APP_VERSION 1.0.0
ENTRYPOINT ["nginx"]
CMD ["-g", "daemon off;"]


# ##############
# EXEMPLO 004
# ##############
# 1. Inclusão da instrução ADD
# 2. Inclusão da instrução HEALTHCHECK
# 2.1. É uma boa prática incluir o HEALTHCHECK no Dockerfile (por mais que se esteja utilizando Kubernetes com Probes)
# 3. Adição do "curl" nos pacotes a serem baixados na instrução RUN
FROM ubuntu:18.04
LABEL maintainer="jeferson@linuxtips.io"
RUN apt-get update && apt-get install nginx curl -y && rm -rf /var/lib/apt/lists/*
EXPOSE 80
ADD node_exporter-1.6.0.linux-amd64.tar.gz /root/node-exporter
COPY index.html /var/www/html/
WORKDIR /var/www/html
ENV APP_VERSION 1.0.0
ENTRYPOINT ["nginx"]
CMD ["-g", "daemon off;"]
HEALTHCHECK --timeout=2s CMD curl -f localhost || exit 1

# ===================================
#           MULTISTAGE
# ===================================

# #############
# EXEMPLO 005
# #############
# 1. Inclusão da image do `golang` no from
# 2. Criação da instrução `COPY` pegando todo o conteúdo do diretório origem ( . ) para o destino ( ./ )
# 2.1 Neste cenário o COPY pegará todo o conteúdo do diretório do `Dockerfile` e enviar para o diretório raiz do container (diretório informado no WORKDIR)

FROM golang:1.18
WORKDIR /app
COPY . ./
RUN go mod init hello
RUN go build -o /app/hello
CMD ["/app/hello"]


# #############
# EXEMPLO 006
# #############
# 1. Inclusão do comando "as" na primeira instrução FROM
# 1.1 Este comando indica que agora o produto deste primeiro bloco do Dockerfile será uma imagem de nome "imageBuild"
# 2. Inclusão de nova instrução FROM indicando uma nova seção do Dockerfile e sua imagem base
# 2.1 Isso indica que agora existe uma MULTISTAGE e que tudo acima se refere à primeira Stage e tudo que está abaixo à segunda STAGE
# 3. Inclusão da instrução COPY copiando informações da primeira stage para a segunda
# 3.1 O comando "--from" indica stage/imagem de origem onde estou buscando a respectiva informação
FROM golang:1.18 as imageBuild
WORKDIR /app
COPY . ./
RUN go mod init hello
RUN go build -o /app/hello

FROM alpine:3.15.9
COPY --from=imageBuild /app/hello /app/hello
CMD ["/app/hello"]


# ===================================
#           ARG e ENV
# ===================================

# #############
# EXEMPLO 007
# #############
# 1. Inclusão da instrução ENV indicando as variáveis de ambiente

FROM golang:1.18 as imageBuild1
WORKDIR /app
COPY . ./
RUN go mod init hello
RUN go build -o /app/hello

FROM alpine:3.15.9
COPY --from=imageBuild1 /app/hello /app/hello
ENV APP="hello_world"
CMD ["/app/hello"]


# #############
# EXEMPLO 008
# #############
# 1. Inclusão da instrução ARG indicando o argumento a ser criado no BUILD
# 2. Inclusão da instrução RUN utilizando o ARG criado

FROM golang:1.18 as imageBuild2
WORKDIR /app
COPY . ./
RUN go mod init hello
RUN go build -o /app/hello

FROM alpine:3.15.9
COPY --from=imageBuild2 /app/hello /app/hello
ENV APP="hello_world"
ARG GIROPOPS="Strigus"
RUN echo "O giropops é: $GIROPOPS"
CMD ["/app/hello"]

# #############
# EXEMPLO 009
# #############
# 1. Inclusão de nova instrução ENV criado a variável "GIRIPOPSENV" e setando o valor dela com o ARG "GIRIPOPS"

FROM golang:1.18 as imageBuild3
WORKDIR /app
COPY . ./
RUN go mod init hello
RUN go build -o /app/hello

FROM alpine:3.15.9
COPY --from=imageBuild3 /app/hello /app/hello
ENV APP="hello_world"
ARG GIROPOPS="Strigus"
ENV GIROPOPSENV=$GIROPOPS
RUN echo "O giropops é: $GIROPOPS"
CMD ["/app/hello"]

# ===================================
#           VOLUME
# ===================================

# #############
# EXEMPLO 010
# #############
# 1. Inclusão da instrução VOLUME indicando o diretório do container que deverá ser persistido (no host)

FROM golang:1.18 as imageBuild4
WORKDIR /app
COPY . ./
RUN go mod init hello
RUN go build -o /app/hello

FROM alpine:3.15.9
COPY --from=imageBuild4 /app/hello /app/hello
ENV APP="hello_world"
ARG GIROPOPS="Strigus"
ENV GIROPOPSENV=$GIROPOPS
RUN echo "O giropops é: $GIROPOPS"
VOLUME /app/dados
CMD ["/app/hello"]