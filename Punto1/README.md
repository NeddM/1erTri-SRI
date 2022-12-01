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

Y también vamos a crear las carpetas `centro.intranet` y `departamentos.centro.intranet`.

```bash
sudo mkdir /var/www/centro.intranet
sudo mkdir /var/www/departamentos.centro.intranet
```

Ahora, vamos a asignarle derechos al usuario, usando la variable de entorno `$USER`.

```bash
sudo chown -R $USER:$USER /var/www/centro.intranet
sudo chown -R $USER:$USER /var/www/departamentos.centro.intranet
```

Y ahora, configuramos cada uno de los VirtualHost para que funcione correctamente.
Dentro de `centros.intranet`:

```apache
<VirtualHost *:80>
    ServerName centro.intranet
    ServerAlias www.centro.intranet
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/centro.intranet
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Y también dentro de `departamentos.centros.intranet`:

```apache
<VirtualHost *:80>
    ServerName departamentos.centro.intranet
    ServerAlias www.departamentos.centro.intranet
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/departamentos.centro.intranet
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

A continuación, habilitaremos los nuevos VirtualHosts:

```bash
sudo a2ensite centro.intranet
sudo a2ensite departamentos.centro.intranet
```

Y deshabilitamos el VirtualHost predeterminado de Apache:

```bash
sudo a2dissite 000-default
```

Ya estaría todo listo. Ahora podemos probar que funciona corractamente, creando un index.html en `/var/www/centro.intranet/index.html`:

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=edge" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>Document</title>
    </head>
    <body>
        <h1>Index</h1>
        <p>centro.intranet</p>
    </body>
</html>
```

Y creamos otro index en `/var/wwww/departamentos.centro.intranet/index.html`:

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=edge" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>Document</title>
    </head>
    <body>
        <h1>Index</h1>
        <p>departamentos.centro.intranet</p>
    </body>
</html>
```

Y listo, ya estarían creados nuestros Virtual Hosts.
