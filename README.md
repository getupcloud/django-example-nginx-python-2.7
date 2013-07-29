openshift-nginx-python-2.7
==========================

Este quickstart cria uma aplicação wsgi [python-2.7](https://github.com/openshift/openshift-community-cartridge-python-2.7),
junto com um servidor nginx.

Arquivos estáticos são servidos pelo nginx nos diretórios `$OPENSHIFT_REPO_DIR/wsgi/static/` e `$OPENSHIFT_REPO_DIR/wsgi/static/media/`.

O objetivo deste quickstart é criar um ambiente mínimo para rodar django com python-2.7 de forma transparente, permitindo que a aplicação
execute no modo `DEBUG = False`. [Entenda por que isso é um problema.](https://docs.djangoproject.com/en/1.5/howto/static-files/)

Como usar
=========

Para criar a aplicação, você precisa antes:

* Registrar-se em [getupcloud.com](http://getupcloud.com)
* Instalar o [cliente rhc](https://getup.zendesk.com/entries/23056511)

Agora basta criar a aplicação:

```sh
$ rhc app create -a wsgi -t python-2.7 \
  --from-code git://github.com/getupcloud/openshift-nginx-python-2.7.git
$ cd wsgi/
```

Abra o arquivo `setup.py` e inclua suas dependências, caso necessário.
Por exemplo, pra instalar Django, o arquivo deve ficar:

```python
from setuptools import setup

setup(name='YourAppName', version='1.0',
      description='OpenShift Python-2.7 Community Cartridge based application',
      author='Your Name', author_email='ramr@example.org',
      url='http://www.python.org/sigs/distutils-sig/',

      #  Uncomment one or more lines below in the install_requires section
      #  for the specific client drivers/modules your application needs.
      install_requires=['greenlet', 'gevent',
                        #  'MySQL-python',
                        #  'pymongo',
                        #  'psycopg2',
                        'Django>=1.5',        # <----- nova dependencia
      ],
     )
```

Após o primeiro push, os pacotes são instalados e a aplicação inicia a execução.

```sh
$ git push
```

Pronto, verifique seu app em http://wsgi-{namespace}.getup.io

Django
======

O banco de dados default é o sqlite e será criado onde foi definido no settings.py

O usuário admin será criado (caso não exista) pelo código no arquivo wsgi/apllication. Inicialmente o login e senha do admin foram definidos como admin e admin.

A variável `install_requires` no arquivo setup.py lê as dependências do arquivo requirements.txt

Os arquivos estáticos serão servidos pelo nginx (config/nginx.conf linhas 54 e 58)
* MEDIA  -> $OPENSHIFT_REPO_DIR/wsgi/static/media/
* STATIC -> $OPENSHIFT_REPO_DIR/wsgi/static/ 

Utilize o script scripts/local.sh para configurar e baixar as dependências localmente e scripts/runserver.sh para rodar o servidor django local.

TODO
====

* Incluir binário estático do nginx OU compilar durante o deploy (muito lento)
* Criar quickstart de django a partir deste


PR são bem-vindos ;P
