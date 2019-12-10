# Practica_image

## **Configuración de desarrollo local Django, Celery y Redis**

Se llevara a cabo el proyecto de crear un servicio web mediante la utilizacion de **Redis** con las paqueterias de **Django** y las tareas se ejecutaran por medio de colas a traves de **Celery**

Lo primero que realizamos fue descargar el archivo de redis, después de que se descargó el archivo lo que realizamos fue ejecutarlo o en otras palabras entrar a la consola y abrir el archivo e instalarlo. Después fue ejecutar una línea de código la cual era **_redis-server_** donde el servidor se encendía (Siempre debe de estar encendido).

Después de haber instalado y ejecutado el servidor de Redis creamos una carpeta nueva de manera local llamada image-parroter. Después de eso dentro de la carpeta ejecutamos el comando **_python3 -m venv venv_** para poder tener una carpeta llamada venv dentro del image-parroter, si en dado caso no teníamos  instalado el venv el comando que debíamos ejecutar era el pip3 install venv.
Después de haber ejecutado el comando anterior, ejecutamos otra línea de código que es la siguiente: **_source venv/bin/activate_**.

Con el entorno visual activado es decir: que este el codigo **_source venv/bin/activate_** esté ejecutado podremos instalar el django celery y redis mediante estos comandos. **_pip install Django Celery redis Pillow django-widget-tweaks_**. Después de eso ejecutamos la siguiente línea de código **_pip freeze > requirements.txt_** para poder tener todos los requerimientos que se nos pide.

## **Configurando el Proyecto Django**

Continuando, creó un proyecto Django llamado image_parroter y luego una aplicación Django llamada thumbnailer.
Después lo que realizamos fue un comando para poder ver el árbol de carpetas que llevamos ejecutadas hasta ahora.

## **Creamos los archivos .py**

El primer archivo o programa  lo creamos dentro de la segunda carpeta de image-parroter el programa se agregó con el nombre celery.py , lo siguiente es lo que contenía la carpeta:
 ```python
 image_parroter/image_parroter/celery.py 
 os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'image_parroter.settings')

celery_app = Celery('image_parroter')
celery_app.config_from_object('django.conf:settings', namespace='CELERY')
celery_app.autodiscover_tasks()

Despues creamos un nuevo archivo de settings.py donde lo que vendra las siguientes lineas de codigo.

# image_parroter/image_parroter/settings.py

# celery
CELERY_BROKER_URL = 'redis://localhost:6379'
CELERY_RESULT_BACKEND = 'redis://localhost:6379'
CELERY_ACCEPT_CONTENT = ['application/json']
CELERY_RESULT_SERIALIZER = 'json'
CELERY_TASK_SERIALIZER = 'json'
```
Despues creamos un nuevo archivo de **__init__**.py donde lo que vendra las siguientes lineas de codigo.
```python
# image_parroter/image_parroter/__init__.py

from .celery import celery_app

__all__ = ('celery_app',)
```
Despues creamos un nuevo archivo de **_tasks.py_** donde lo que vendra las siguientes lineas de código dentro de la carpeta thumbnailer.
```python
# image_parroter/thumbnailer/tasks.py

from celery import shared_task

@shared_task
def adding_task(x, y):
    return x + y
```
Después creamos un nuevo archivo setting.py de donde lo que vendrá las siguientes líneas de código están se usarán para agregar al proyecto todas las carpetas necesarias para llevar a su ejecución.
```python
# image_parroter/image_parroter/settings.py

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'thumbnailer.apps.ThumbnailerConfig',
    'widget_tweaks',
]
```
## **Iniciar el servidor**
Primero que nada tenemos que ejecutar el comando para abrir redis ejecutando el comando **_redis-server_** donde se vera como inicia el servidor para hacer llegar al cliente la informacion.

Después debemos ejecutar el comando(venv) **_$ celery worker -A image_parroter --loglevel=info_** desde el virtual es decir cuando muestra el venv, esto para iniciar las tareas de celery, que se ejecutan mediante colas por medio de las paqueterias de Django.

Después de haber ejecutado esos comandos debemos ejecutar unos últimos desde otra terminal. Este comando se ejecuta desde el image-parroter en el que se enuentre el **_manage.py_** si no no podra ejecutarse y también debemos tener copiado el código del **html** dentro de los templates.

## **Crear miniaturas de imágenes dentro de una tarea de celery**
En esta sección se agrega un código a **_# image_parroter/thumbnailer/tasks.py_**
```python
# image_parroter/settings.py
# thumbnailer/views.py
# thumbnailer/urls.py
```
Se crea la carpeta (venv) **_$ mkdir -p thumbnailer/templates/thumbnailer_**

En la carpeta creada se crea un documento el cual contendrá el código para el funcionamiento de la página el cual llamaremos **_home.html_**
Una vez creado el documento procedemos a correr el siguiente comando (venv) **_$ python manage.py runserver_**
EStando en  funcionamiento el servidor procedemos a abrir el documento **_home.html_** para que nos redirige a la página, o bien al ejecutar desde terminal el comando **_python manage.py runserver_** mostrara la IP de la pagina solo debemos abrirla para visualizarla.








