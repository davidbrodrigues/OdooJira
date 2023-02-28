# Usando docker-compose con Odoo

Docker Compose es una herramienta para definir y ejecutar aplicaciones de Docker de varios contenedores. 
En Compose, se usa un archivo YAML para configurar los servicios de la aplicación. Después, con un solo 
comando, se crean y se inician todos los servicios de la configuración.

# Ejemplo de mi docker-compose


Aquí esta el `docker-compose.yml` que crea todo lo que necesito para utilizar odoo:

```yaml
version: '3'
services:
  web:
    image: odoo:latest
    depends_on:
      - db
    ports:
      - "8069:8069"
    volumes:
      - /home/dam2a/Escritorio/odoo/srv/odoo:/var/lib/odoo
      - /home/dam2a/Escritorio/odoo/srv/odoo/config:/etc/odoo
  db:
    image: postgres:latest
    environment:
      - POSTGRES_DB=postgresjira
      - POSTGRES_PASSWORD=odoo
      - POSTGRES_USER=odoo
    ports:
      - "5432:5432"
    volumes:
      - /home/dam2a/Escritorio/odoo/sql:/var/lib/postgresql/data

```

# Breve explicacion de los pasos -> 

Para comenzar, especificamos la versión del documento.

```
version: '3'
```

Luego lo separaremos en dos partes, es decir los dos servicios que utilizaremos.
El servicio de odoo y el servicio de postgresql pra la base de datos.
En mi caso los llamo web y db:

```
services:
  web:
    ...
  db:
    ...
```

Para comenzar especifico en ambos casos la versión de la imagen, en mi caso la más reciente.

```
image: odoo:latest
...
image: postgres:latest

```

En el paratado de puertos mapeo los puertos de ambos servicios.

```
Para odoo->
    ports:
      - "8069:8069"
Para sql->
    ports:
      - "5432:5432"
```

El apartado volumes, es para mapear las carpetas de configuración de ambos servicios localmente
para en un futuro poder hacer modificaciones y saber donde se situan.

```
Para odoo->
    volumes:
      - /home/dam2a/Escritorio/odoo/srv/odoo:/var/lib/odoo
      - /home/dam2a/Escritorio/odoo/srv/odoo/config:/etc/odoo
Para sql->
    volumes:
      - /home/dam2a/Escritorio/odoo/sql:/var/lib/postgresql/data
```

Y por último para el apartado de envroment en sql, pondremos los datos de acceso a la misma y su nombre.

```
    environment:
      - POSTGRES_DB=postgresjira
      - POSTGRES_PASSWORD=odoo
      - POSTGRES_USER=odoo
```