### Workshop 3 - Sistemas Distribuidos
## Nombre: Laura Aragón
## Código: A00268532
## Perfil Github: https://github.com/LauraAragon

El Workshop 3 consiste en el aprovisionamiento de un ambiente compuesto por un servidor web Apache y un servidor de base de datos MySQL,y de una aplicación web que realiza consultas a la base de datos y despliega los datos obtenidos a través de la consulta.

Para construir el contenedor del servidor de base de datos MySQL se utilizó el DockerFile presentado a continuación:

```
FROM ubuntu

ADD files/ /temp/

RUN apt-get update -y --fix-missing

RUN apt-get install expect -y

WORKDIR /temp

RUN chmod +x install_mysql

RUN chmod +x configure_mysql.sh

RUN ./configure_mysql.sh

EXPOSE 3306

CMD  /etc/init.d/mysql start && tail -f /var/log/mysql/error.log
```

Para construir el contenedor del servidor web Apache se utilizó el DOckerFile presentado a continuación:

```
FROM ubuntu-web:latest

RUN apt-get update && apt-get install apache2 -y

RUN apt-get install php libapache2-mod-php php-mcrypt php-mysql -y

ADD html/index.php /var/www/html/index.php

EXPOSE 80

CMD service ssh start && service apache2 start && tail -f /var/log/apache2/access.log
```

Para la ejecución de los contenedores (servidor web y servidor base de datos) se hizo uso de Docker Compose. El archivo de configuración se muestra a continuación:

```
version: '2'
services:
  web:
    build: ./web-server/
    ports:
     - "8080:80"
     - "2222:22"
  db:
   build: ./db-server/
   ports:
    - "3306:3306"
```

Para levantar el ambiente en su totalidad se utiliza el siguiente comando, el cual debe ser ejecutado dentro de la carpeta que contiene el archivo docker-compose.yaml:

```bash
docker-compose up
```

Funcionamiento de la aplicación:

![alt text](https://k60.kn3.net/8/F/0/3/8/B/934.jpg)