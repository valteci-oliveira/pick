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




