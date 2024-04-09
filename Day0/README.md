# Comandos Gerais - Linux Essencials

- `ls -l` Lista arquivos de forma longa*

- `tree` Lista a arvore de diretórios visualmente melhor

- `mkdir -p` Permite criar uma arvore de diretórios de uma vez

- `rm` Remove arquivos ou diretórios

- `rm -r` Remove diretório e subdiretórios (como confirmacao em cada diretório)

- `rm -rf` Remove diretórios e subdiretórios sem perguntar **(PERIGOOSO)**

- `rmdir` Realiza a exclusao de diretórios (vazios)

- `cat [NOME ARQUIVO]` Permite realizar a vizualizacao de um arquivo

- `cat -E` Adiciona um caracter indicando o final da linha ***(bom para ajudar na visualizacao de espacos no fim da linha)***

- `zcat` Permite visualizar arquivos gz

- `cp` Permite copiar arquivos

- `cp -r` Copia todos os arquivos e também os diretórios
*`cp arquivo1 arquivo2`* 

<br><br>

# Setup para utilização no PICK2024
Optei por utilizar o `Vagrant` para otimizar o processo de criação do Setup e também para conhecer um pouco mais da ferramenta que até então eu não conhecia.

## Instalação
Segui o processo de instalação padrão informado no site da [Vagrant](https://developer.hashicorp.com/vagrant/install?product_intent=vagrant)

    brew tap hashicorp/tap
    brew install hashicorp/tap/hashicorp-vagrant

<br>

### Ambiente Debian ( :bomb: Abortado  ) 

    vagrant init debian/jessie64
    vagrant up
    vagrant ssh

Inicialmente eu realizar a instalação do vagrantBox da distribuição [Debian](), mas no momento de rodar a VM a mesma apresentou alguns erros como:

1. `No guest additions were detected on the base box for this VM! Guest`
    - Resolvido com a [instalação do plugin **vagrant-vbguest**](https://subscription.packtpub.com/book/cloud-and-networking/9781786464910/1/ch01lvl1sec12/enabling-virtualbox-guest-additions-in-vagrant) (`vagrant plugin install vagrant-vbguest`)

2. `The following SSH command responded with a non-zero exit status` 
    - Este erro não consegui identificar a causa raiz (mesmo após análise dos logs de instalação)
        - Logs extraídos via `vagrant up --debug`

<br>

### Ambiente Ubuntu ( :white_check_mark: Sucesso )

    vagrant init bento/ubuntu-22.04
    vagrant up
    vagrant ssh

Uma vez dentro da VM iniciei o processo de instalação do Docker. <br>
Como eu não era usuário `root` da máquina precisei executar os passos abaixo: 

1. Setar uma nova senha para o root (não consegui identificar a senha que veio por default na VM)
    - `sudo passwd root` - [Referência](https://stackoverflow.com/questions/25758737/vagrant-login-as-root-by-default) 

2. Instalar o Docker via curl
    - `curl -fsSL https://get.docker.com/ | bash`

3. Instalar ferramenta para permitir o Docker ser utilizado sem ser via usuário root
    - `dockerd-rootless-setuptool.sh install`

<br>

Feito isso o setup está criado e pronto para utilização!   VAMOOOO!!