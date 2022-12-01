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
