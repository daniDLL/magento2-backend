# Magento 2 Backend
----

## Table of Contents
1. [Historia de Magento](/magento2-backend#historia-de-magento)
1. [Docker](/magento2-backend#docker)
1. [Instalación de Magento](/magento2-backend#instalación-de-magento)
1. [Design Patterns](/magento2-backend#design-patterns)
1. [Modular Structure](/magento2-backend#modular-structure)
1. [Routing](/magento2-backend#routing)
1. [Service Contracts](/magento2-backend#service-contracts)
1. [CLI commands](/magento2-backend#cli-commands)
1. [Management Entities](/magento2-backend#management-entities)
1. [Observers](/magento2-backend#observers)
1. [Plugins](/magento2-backend#plugins)
1. [Proxies](/magento2-backend#proxies)

## Historia de Magento

Magento es una plataforma escrita en php de código abierto orientada para el desarrollo de comercio electrónico. Es usado por aproximadamente uno de cada cuatro comercios electrónicos, en 2017 la situación estaba así:

![Magento Layouts Example](./images/statistics.png "Magento Layouts Example")

Fue lanzado en el 2008 (11 años) por Varien Inc (Ahora Magento Inc) y ha pasado a ser comprado por diversas empresas: en 2011 Ebay compró el 49% y finalmente la totalidad de Magento, pero en 2015 se separó como una compañía independiente al escindirse.

En el 2015 se lanzó la versión 2.0 de la plataforma, cambiándola por completo, en un principio a mejor, sobre el papel... la idea de meter numerosos patrones de diseños para dotar de robustez a la plataforma y estandarizarla fue una completa genialidad a nivel de backend, pero… en el front fue una completa tortura.

En el año 2018 finalmente la compañía fue vendida al actual propietario, Adobe, el cual la adquirió por la cuantía de 1680 millones de dólares.

Se ha ido actualizando la plataforma hasta la actual versión de código estable, la 2.4. En 2020 se ha dejado de dar soporte de manera oficial a las plataformas que funcionen con la versión anterior (Magento 1.x).

## Docker

Docker es un proyecto de código abierto que automatiza el despliegue de aplicaciones dentro de contenedores de software, proporcionando una capa adicional de abstracción y automatización de virtualización de aplicaciones en múltiples sistemas operativos.
​Docker utiliza características de aislamiento de recursos del kernel Linux, tales como cgroups y espacios de nombres (namespaces) para permitir que "contenedores" independientes se ejecuten dentro de una sola instancia de Linux, evitando la sobrecarga de iniciar y mantener máquinas virtuales.

Los contenedores se utilizan como máquinas virtuales extremadamente livianas y modulares. Además, obtiene flexibilidad con estos contenedores: pueden ser creados, copiados y movidos entre entornos con sistemas operativos completamente distintos.

La tecnología Docker usa el kernel de Linux y las funciones de este, como grupos y namespaces, para segregar los procesos, de modo que puedan ejecutarse de manera independiente. El propósito de los contenedores es esta independencia: la capacidad de ejecutar varios procesos y aplicaciones por separado para hacer un mejor uso de su infraestructura y, al mismo tiempo, conservar la seguridad que tendría con sistemas separados.

Las herramientas del contenedor, como Docker, ofrecen un modelo de implementación basado en imágenes. Esto permite compartir una aplicación, o un conjunto de servicios, con todas sus dependencias en varios entornos. Docker también automatiza la implementación de la aplicación (o conjuntos combinados de procesos que constituyen una aplicación) en este entorno de contenedores.

Estas herramientas desarrolladas a partir de los contenedores de Linux, lo que hace a Docker fácil de usar y único, otorgan a los usuarios un acceso sin precedentes a las aplicaciones, la capacidad de implementar rápidamente y control sobre las versiones y su distribución.

Cabe destacar que en Mac o en Windows si que se utlizan máquinas virtuales

### Principales ventajas del uso de Docker:

1. Modularidad: El enfoque Docker para la creación de contenedores se centra en la capacidad de tomar una parte de una aplicación, para actualizarla o repararla, sin necesidad de tomar la aplicación completa.

1. Control de versiones de imágenes y capas: Cada archivo de imagen de Docker se compone de una serie de capas. Estas capas se combinan en una sola imagen. Una capa se crea cuando la imagen cambia. Cada vez que un usuario especifica un comando, como ejecutar o copiar, se crea una nueva capa.
Docker reutiliza estas capas para construir nuevos contenedores, lo cual hace mucho más rápido el proceso de construcción. Los cambios intermedios se comparten entre imágenes, mejorando aún más la velocidad, el tamaño y la eficiencia. El control de versiones es inherente a la creación de capas. Cada vez que se produce un cambio nuevo, básicamente, el usuario tiene un registro de cambios incorporado: control completo de sus imágenes de contenedor.

1. Implementación rápida: Los contenedores basados en Docker pueden reducir el tiempo de implementación de un entorno de horas a segundos. La tecnología de Docker tiene un enfoque granular y controlable, basado en microservicios, que prioriza la eficiencia.


## Instalación de Magento

Para seguir los principales puntos del curso, vamos a seguir un módulo de ejemplo que contiene las partes básicas en el desarrollo de módulos (todo parte back) en Magento 2.

```
git clone https://github.com/daniDLL/magento2-course.git
```

### Steps to install
---

### 1. Init

```
git fetch
git checkout master
```

### 2. Docker Compose UP

```
docker-compose up -d
```

### 3. Enter docker php container

```
docker exec -it magento-php-course-2020 bash
```

### 4. Composer install

```
composer install
```

### 5. Magento 2 repository access keys

```
Public key: f7e114d972963174c89e1af64e761a29
Private key: ec51d43d89a4d053e57d5e5f79d968f7
```

### 6. Install Magento 2

```
bin/magento setup:install \
--base-url=http://curso.magento.local/ \
--db-host=mysql \
--db-name=magento \
--db-user=magento \
--db-password=magento \
--admin-firstname=admin \
--admin-lastname=admin \
--admin-email=admin@admin.com \
--admin-user=admin \
--admin-password=admin123 \
--language=en_US \
--currency=EUR \
--timezone=Europe/Madrid \
--elasticsearch-host elasticsearch \
--elasticsearch-port 9200 \
--use-rewrites=1
```

### 7. Config app/etc/env.php (outside the container)

```
curl -o app/etc/env.php https://raw.githubusercontent.com/daniDLL/magento2-course/master/app/etc/env.php.bak
```

### 8. Setting file permissions (inside the container)

```
find var generated vendor pub/static pub/media app/etc -type f -exec chmod g+w {} +
```

```
find var generated vendor pub/static pub/media app/etc -type d -exec chmod g+ws {} +
```


> All rights reserved to [Magento2 DevDoc](https://devdocs.magento.com/#/individual-contributors)

> Gracias a [Víctor Jurado Usón](https://github.com/vjuradouson "Víctor Jurado Usón") por la gran introducción y apuntes sobre el curso de Magento 2 - Backend.
