
## PASSOS PARA EXECUTAR APP LOCAL

1. Clonar o nosso repo:
git clone https://github.com/badtuxx/giropops-senhas.git

2. Entrar no diretório
cd giropops-senhas/

3. Realizando o update do apt
apt-get update

4. Instalando o PIP
apt-get install pip

5. Instalando todas as dependencias da nossa app
pip install --no-cache-dir -r requirements.txt

6. apt-get install redis
apt-get install redis

7. Iniciando o Redis
systemctl start redis
systemctl status redis

8. Criando a variavel de ambiente para que a nossa App saiba onde está o Redis
export REDIS_HOST=localhost

9. Iniciando a nossa APp
flask run --host=0.0.0.0


>ERROR:
<br><br>
Traceback (most recent call last):
  File "/usr/local/bin/flask", line 5, in <module>
    from flask.cli import main
  File "/usr/local/lib/python3.8/dist-packages/flask/__init__.py", line 7, in <module>
    from .app import Flask as Flask
  File "/usr/local/lib/python3.8/dist-packages/flask/app.py", line 27, in <module>
    from . import cli
  File "/usr/local/lib/python3.8/dist-packages/flask/cli.py", line 17, in <module>
    from .helpers import get_debug_flag
  File "/usr/local/lib/python3.8/dist-packages/flask/helpers.py", line 14, in <module>
    from werkzeug.urls import url_quote
ImportError: cannot import name 'url_quote' from 'werkzeug.urls' (/usr/local/lib/python3.8/dist-packages/werkzeug/urls.py)


10. Update do Flask
pip install --upgrade Flask


## ATIVIDADES DESAFIO
- Criar um conta no Docker Hub, caso ainda não possua uma.
- Criar uma conta no Github, caso ainda não possua uma.
- Criar um Dockerfile para criar uma imagem de container para a nossa App
- O nome da imagem deve ser SEU_USUARIO_NO_DOCKER_HUB/linuxtips-giropops-senhas:1.0
- Fazer o push da imagem para o Docker Hub, essa imagem deve ser pública
- Criar um repo no Github chamado LINUXtips-Giropops-Senhas, esse repo deve ser público
- Fazer o push do cógido da App e o Dockerfile
- Criar um container utilizando a imagem criada
- O nome do container deve ser giropops-senhas
- Você precisa deixar o container rodando
- O Redis precisa ser um container

Dica: Preste atenção no uso de variável de ambiente, precisamos ter a variável REDIS_HOST no container. Use sua criatividade!