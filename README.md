# Proyecto 1er trimestre SRI.

## Instalación del servidor web Apache.

Realizaremos la instalación de un servidor web Apache. Para ello, usaremos dos dominios meidante el archivo hosts: _centro.intranet_ y _departamentos.centro.intranet_. El primero servirá el contenido meidante Wordpress y el segundo una aplicación en Python.

Para empezar, vamos a actualizar nuestro sistema operativo Ubuntu, para ello, nos dirigimos a la terminal y escribimos:

```bash
sudo apt update
```

Luego, procederemos a instalar apache2:

```bash
sudo apt install apache2
```

##### Configuramos el firewall para permitir el tráfico web

Lo siguiente que tenemos que hacer es ajustar el firewall, para permitir el tráfico web. Esto permite el paso de los protocolos **HTTP** y **HTTPS**.
Para ello, escribimos:

```bash
sudo ufw app list
```

A nosotros nos interesa instalar el `Apache Full`. Con el siguiente comando podemos ver más información sobre cada opción que nos ofrece el Firewall.

```bash
sudo ufw app info "Apache Full"
```

La consola nos devolverá lo siguiente:

```bash
Output
Profile: Apache Full
Title: Web Server (HTTP,HTTPS)
Description: Apache v2 is the next generation of the omnipresent Apache web
server.

Ports:
  80,443/tcp
```

Como podemos comprobar, es lo que buscamos, ya que "Apache Full" nos abre los puertos `80 http` y `443 https`. Por lo tanto, escribimos:

```bash
sudo ufw allow "Apache Full"
```

La consola nos devolverá:

```
Rules updated
Rules updated (v6)
```

Ahora podemos probar si todo funciona, vamos a nuestro navegador favorito, y escribimos la dirección de loopback en la barra de direcicones:

```
http://127.0.0.1
```

![Apache2, índice por defecto.](/img/1.png)

## Activar los módulos necesarios.

Debemos activar los módulos necesarios para ejecutar PHP y acceder a MySQL.

Para ello, vamos a instalar primeramente el MySQL. Escribimos en la terminal:

```bash
sudo apt install mysql-server
```

Ya estaría listo. Para iniciar MySQL escribimos en la terminal:

```bash
sudo mysql
```

Si todo ha ido bien, se nos devolverá algo así:

```bash
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 5
Server version: 5.7.34-0ubuntu0.18.04.1 (Ubuntu)

Copyright (c) 2000, 2021, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```

Para salir de MySQL, escribimos:

```sql
mysql> exit
```

---

Ahora vamos a proceder a instalar PHP, para ello, vamos a escribir en la terminal:

```bash
sudo apt install php libapache2-mod-php php-mysql
```

## Instala y configura Wordpress.

Debemos instalar y configurar Wordpress.

## Activar el módulo "wsgi".

Activar el módulo "wsgi" para permitir la ejecución de aplicaciones Python.

## Crear y desplegar una aplicación.

Crear y desplegar una pequeña aplicación Python para comprobar que funciona correctamente.

## Protegemos el acceso a la aplicación Python.

Adicionalmente, protegeremos el acceso a la palicación Python mediante autenticación.

## Instalar y configurar awsat.

## Instalar un segundo servidor.

Vamos a instalar un segundo servidor, por ejemplo, _nginx_ o _lightpd_, bajo el dominio "servidor2.centro.Intranet". Debes configurarlo para que sirva en el puerto 8080 y haz los cambios necesarios para ejecutar PHP. Instala Phpmyadmin.
