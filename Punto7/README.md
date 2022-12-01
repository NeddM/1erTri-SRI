## Instalar y configurar AWStats.

Habilitamos el módulo CGI en Apache.

```bash
sudo a2enmod cgi
sudo /etc/init.d/apache2 restart
```

A continuación, abriremos el archivo de configuración de AWStats, para cambiar algunas cosas.
Copiamos la plantilla de configuración predefinida, con el siguiente comando:

```bash
sudo cp /etc/awstats/awstats.conf /etc/awstats/awstats.centro.intranet.conf
```

Y lo abrimos con un editor de texto...

Vamos a modificar la siguiente informacion.

```
LogFile=”/var/log/apache2/access.log”  (buscar y verificar que queda así)
SiteDomain=”centro.intranet”
HostAliases=”centro.intranet localhost 127.0.0.1”
AllowToUpdateStatsFromBrowser=1
```

Despues de realizar los cambios mencionados anteriormente, introducimos el siguiente comando en la consola de Linux:

```bash
sudo /usr/lib/cgi-bin/awstats.pl -config=centro.intranet -update
```

Y para terminar nuestra configuración, ejecutaremos los siguientes comandos:

```bash
sudo cp -r /usr/lib/cgi-bin /var/www/html/
sudo chown -R www-dat
www-data /var/www/html/cgi-bin/
sudo chmod -R 755 /var/www/html/cgi-bin/
```

Ya estaría todo listo, vamos a escribir lo siguiente en el navegador:

```url
http://127.0.0.1/cgi-bin/awstats.pl?config=centro.intranet
```
