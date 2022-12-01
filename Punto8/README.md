## Instalar un segundo servidor.

https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-20-04-es

Vamos a instalar un segundo servidor, por ejemplo, _nginx_ o _lightpd_, bajo el dominio "servidor2.centro.Intranet". Debes configurarlo para que sirva en el puerto 8080 y haz los cambios necesarios para ejecutar PHP. Instala Phpmyadmin.

Para empezar, tenemos que actualizar nuestro sistema Ubuntu, y también vamos a instalar _nginx_:

```bash
sudo apt update
sudo apt install nginx
```

Una vez instalado, le daremos privilegios a nginx en el firewall:

```bash
sudo ufw allow 'Nginx HTTP'
```

Y comprobaremos que todo va correctamente:

```bash
systemctl status nginx
```

Ahora, vamos a configurar los DNS. Para ello, vamos a crear la siguiente carpeta:

```bash
sudo mkdir -p /var/www/servidor2.centro.intranet/html
```

Y le asignaremos la propiedad del directorio:

```bash
sudo chown -R $USER:$USER /var/www/servidor2.centro.intranet/html
sudo chmod -R 755 /var/www/servidor2.centro.intranet
```

Creamos un index para nuestra web...

```bash
nano /var/www/servidor2.centro.intranet/html/index.html
```

Y tambien crearemos la directiva en el servidor:

```bash
sudo nano /etc/nginx/sites-available/servidor2.centro.intranet
```

Dentro escribimos:

```nginx
server {
        listen 8080;
        listen [::]:8080;

        root /var/www/servidor2.centro.intranet/html;
        index index.html index.htm index.nginx-debian.html;

        server_name servidor2.centro.intranet www.servidor2.centro.intranet;

        location / {
                try_files $uri $uri/ =404;
        }
}
```

También tenemos que crear un `sites-enabled`.

```bash
sudo ln -s /etc/nginx/sites-available/servidor2.centro.intranet /etc/nginx/sites-enabled/
```

Por último, tenemos que descomentar una linea en el archivo de configuración de `nginx.conf`.

```
...
http {
    ...
    server_names_hash_bucket_size 64;
    ...
}
...
```

Vamos a probar que no hayan errores de sintaxis:

```bash
sudo nginx -t
```

Y si no hay fallo, vamos reiniciar el servicio de nginx:

```bash
sudo systemctl restart nginx
```

¡Y ya tenemos listo nuestro servidor **NGINX**!
