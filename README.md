# Proyecto 1er trimestre SRI.

## Instalación del servidor web Apache.

Realizaremos la instalación de un servidor web Apache. Para ello, usaremos dos dominios meidante el archivo hosts: _centro.intranet_ y _departamentos.centro.intranet_. El primero servirá el contenido mediante Wordpress y el segundo una aplicación en Python.

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

Para ello, tenemos que entrar en MySQL. Ya que WordPress usa MySQL para administrar y guardar la información.

Para entrar en MySQL escribimos en la terminal:

```bash
sudo mysql -u root -p
```

Y ahora, podemos crear nuestra base de datos Wordpress. Para ello, escribimos dentro de la terminal MySQL:

```sql
CREATE DATABASE wordpress DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;
```

Ya tendremos creada nuestra base de datos para Wordpress. Ahora lo siguiente que vamos a hacer es separar las cuentas de usuarios para operar con la base de datos. El mio se llamará **wordpressnedd**, y la contraseña será **password**.

```
CREATE USER 'wordpressnedd'@'%' IDENTIFIED WITH mysql_native_password BY 'password';
```

Ahora, vamos a instalar las extensiones PHP adicionales, que son necesarias para el buen funcionamiento de Wordpress. Para ello, escribimos en la terminal los siguientes comandos:

```bash
sudo apt update
sudo apt install php-curl php-gd php-mbstring php-xml php-xmlrpc php-soap php-intl php-zip
```

Una vez realizado esto, reiniciamos el servicio de Apache:

```bash
sudo systemctl restart apache2
```

Por último, necesitaremos descargar Wordpress, para ello, en la terminal escribimos:

```bash
cd /tmp
curl -O https://wordpress.org/latest.tar.gz
```

Eso nos descargará un archivo comprimido, ahora vamos a proceder a descomprimirlo:

```bash
tar xzvf latest.tar.gz
```

Ahora vamos a mover estos archivos al document root. Podemos hacerlo dentro de un `.htaccess`.

```bash
touch /tmp/wordpress/.htaccess
```

También vamos a copiar la configuración predeterminada al archivo que leerá Wordpress:

```bash
cp /tmp/wordpress/wp-config-sample.php /tmp/wordpress/wp-config.php
```

Y también podemos crear un `upgrade` directory, así Wordress no tendrá problemas sobre permisos.

```bash
mkdir /tmp/wordpress/wp-content/upgrade
```

Y ya podríamos copiar todo el contenido del directorio hacia el document root:

```bash
sudo cp -a /tmp/wordpress/. /var/www/wordpress
```

Ahora vamos a proceder a configurar el directorio Wordpress. Un paso importante es ajustar bien los permisos.
Podemos elevar los privilegios usando el comando `chown`, que nos permite cambiar el propietario del archivo. Para ello escribimos:

```bash
sudo chown -R www-data:www-data /var/www/wordpress
```

Y lo siguiente que haremos será usar el comando `find`, para establecer bien los permisos de los directorios y los archivos:

```bash
sudo find /var/www/wordpress/ -type d -exec chmod 750 {} \;
sudo find /var/www/wordpress/ -type f -exec chmod 640 {} \;
```

Ahora tenemos que hacer algunos cambios en la configuración principal de Wordpress. Para conseguir los valores seguros que genera Wordpress escribimos:

```bash
curl -s https://api.wordpress.org/secret-key/1.1/salt/
```

Abrimos el archivo de configuración de Wordpress:

```bash
sudo nano /var/www/wordpress/wp-config.php
```

Y dentro del archivo de configuración, metemos nuestras claves privadas:

```bash
define('AUTH_KEY',         'm.6 Gx*h5rV)c}|t-i;}=%]rd%Fh_%65kJ GFw,9f GXwP*R*rljtx9.qzB+]`BU');
define('SECURE_AUTH_KEY',  't+_[<%:yRZy~MLuM $M:dy2k(0^cN:<sw$97S/V<Q?cd^V(:COwT|fY+IL&=|Q[j');
define('LOGGED_IN_KEY',    '7[K.z:oZ)O:c<5+bN;+PoOn+`=+ -:%C7(9.!Hm&*WVxrbB<)iEUwIfmA6dh0Tt^');
define('NONCE_KEY',        '1jBUgt>;@ g@,(`5tT5z-u^~PG!*>/ ]T*v3uI@JRah)qcn3+nT8|d71xU>kvjac');
define('AUTH_SALT',        '3VeXTx~Wo7J_6liTY}):O$R-F4GEE)S*+adUxxAI.GQB)&{_=Qe_UC|:j],TqL&#');
define('SECURE_AUTH_SALT', '3Dg@iYh0/?t/T-|JK+au`G-,O{h/>p7WE9 aBDN^-Z:{=+-#p0Bl;{P73#[)kd7]');
define('LOGGED_IN_SALT',   '!1S1f,^*6J(AS9z;Xuzz-)K/lsOA]i1_0Ip)~%VA:IbzzivXe(_h5X0X6,m=:8%R');
define('NONCE_SALT',       '1(I}2A5kJLFg}tE<}p)(h8VPM`|q.D hM/|Do(?f(obSk~koTST- hR!oU8)-Pw_');
```

## Activar el módulo "wsgi".

Activar el módulo "wsgi" para permitir la ejecución de aplicaciones Python.

Lo primero que haremos será actualizar el sistema e instalar algunas librerias necesarias:

```bash
sudo apt-get update
sudo apt-get install apache2 apache2-utils libexpat1 ssl-cert python3
```

Lo siguiente que haremos, será descargar el mod_wsgi:

```url
https://codeload.github.com/GrahamDumpleton/mod_wsgi/tar.gz/4.9.4
```

Y lo descomprimimos:

```bash
tar xvfz mod_wsgi-X.Y.tar.gz
```

También vamos a bajar la librería de wsgi para Apache:

```bash
sudo apt install libapache2-mod-wsgi-py3
```

Ahora, podemos configurarlo desde el archivo `configure` vamos a configurarlo:

```bash
nano configure
```

Y para activarlo en apache, nos dirigimos a nuestro virtual host, en `/etc/apache2/sites-enabled/000-default.conf`, y escribimos lo siguiente:

```apache
a2enmod wsgi
```

![Activando el módulo wsgi](/img/3.png)

## Crear y desplegar una aplicación.

Crearemos y desplegaremos una pequeña aplicación Python para comprobar que funciona correctamente.

Para ello, vamos a instalar el repositorio _uwsgi_.

```bash
pip install uwsgi
```

Y vamos a crear un _Hola Mundo_.
Esta aplicación es simple, es una función que; si la respuesta del servidor es 200 (Exitosa), nos devolverá un **Hola mundo**.

```python
def application(env, start_response):
    start_response('200 OK', [('Content-Type','text/html')])
    return [b"Hola Mundo"]
```

Y ya podremos empezar a usar nuestra aplicación:

```
uwsgi --http :9090 --wsgi-file foobar.py
```

![Ejecutando uwsgi](/img/4.png)

Incluso, podríamos añadir más nucleos e hilos de procesador a nuestra aplicación:

```
uwsgi --http :9090 --wsgi-file foobar.py --master --processes 4 --threads 2
```

## Protegemos el acceso a la aplicación Python.

Adicionalmente, protegeremos el acceso a la palicación Python mediante autenticación.

Esto lo haremos añadiendo varias lineas a nuestro archivo _Virtual Host_, en `/etc/apache2/sites-available/000-default.conf`:

```apache
AuthType Digest
AuthName "Top Secret"
AuthDigestProvider wsgi
WSGIAuthUserScript /usr/local/wsgi/scripts/auth.wsgi
Require valid-user
```

## Instalar y configurar awsat.

## Instalar un segundo servidor.

Vamos a instalar un segundo servidor, por ejemplo, _nginx_ o _lightpd_, bajo el dominio "servidor2.centro.Intranet". Debes configurarlo para que sirva en el puerto 8080 y haz los cambios necesarios para ejecutar PHP. Instala Phpmyadmin.
