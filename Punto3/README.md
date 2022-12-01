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
