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
