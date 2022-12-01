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
