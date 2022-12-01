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
