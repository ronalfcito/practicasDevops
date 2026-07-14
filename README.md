# Proyecto Docker DevOps — Aplicación Flask + PostgreSQL

## Propósito

Este repositorio contiene una aplicación web desarrollada con Flask que permite gestionar productos en una base de datos PostgreSQL. El objetivo de este proyecto es mostrar cómo construir, ejecutar y orquestar servicios con Docker y Docker Compose de forma práctica.

La aplicación incluye:

- Un backend en Flask
- Una base de datos PostgreSQL
- Un flujo completo de Dockerización
- Un ejemplo sencillo de despliegue con contenedores

---

## 1. Introducción rápida

Docker permite empaquetar una aplicación y todas sus dependencias en contenedores, lo que facilita su ejecución en cualquier entorno. En este proyecto, el contenedor web ejecuta la aplicación Flask y el contenedor de base de datos ofrece PostgreSQL para almacenar los registros.

Esto es útil porque:

- No dependes de instalaciones locales complejas
- El entorno es reproducible
- El despliegue es más rápido y consistente

---

## 2. Estructura del proyecto

```text
nuevoProyectoDV/
├── app/
│   └── app.py
├── Dockerfile
├── docker-compose.yml
├── requirements.txt
├── setup.py
└── README.md
```

### Archivos principales

- app/app.py: aplicación Flask con rutas para listar, crear y eliminar productos.
- Dockerfile: define cómo construir la imagen de la aplicación.
- docker-compose.yml: define los servicios web y base de datos.
- requirements.txt: dependencias de Python.
- setup.py: configuración del paquete Python.

---

## 3. ¿Qué hace esta aplicación?

La aplicación permite:

- Mostrar una página web con los productos registrados
- Crear nuevos productos
- Eliminar productos existentes
- Guardar los datos en PostgreSQL

La app se conecta a la base de datos usando variables de entorno como:

- DB_HOST
- DB_NAME
- DB_USER
- DB_PASSWORD

---

## 4. Requisitos previos

Antes de ejecutar el proyecto necesitas tener instalado:

- Docker
- Docker Compose

Verifica que Docker esté funcionando con:

```bash
docker --version
docker compose version
```

---

## 5. Ejecutar el proyecto con Docker Compose

Desde la raíz del proyecto, ejecuta:

```bash
docker compose up --build
```

Este comando:

- Construye la imagen de la aplicación
- Levanta el contenedor web
- Levanta el contenedor de PostgreSQL
- Conecta ambos servicios automáticamente

### Acceder a la aplicación

Abre en el navegador:

```text
http://localhost:5000
```

---

## 6. Servicios definidos en Docker Compose

El archivo docker-compose.yml define dos servicios:

### Servicio web

- Ejecuta la aplicación Flask
- Expone el puerto 5000
- Se conecta al servicio de base de datos

### Servicio db

- Ejecuta PostgreSQL 15
- Mantiene los datos en un volumen persistente
- Expone el puerto 5432

---

## 7. ¿Qué es un Dockerfile?

Un Dockerfile es un archivo de texto que le indica a Docker cómo construir una imagen de la aplicación.

En este proyecto, el Dockerfile utiliza Python 3.10 slim como imagen base y realiza lo siguiente:

```dockerfile
FROM python:3.10-slim

WORKDIR /workspace

COPY . /workspace

RUN pip install --no-cache-dir -r requirements.txt
RUN pip install .

EXPOSE 5000

CMD ["ejecutar-crud"]
```

### Explicación de las instrucciones

- FROM: indica la imagen base.
- WORKDIR: define el directorio de trabajo dentro del contenedor.
- COPY: copia los archivos del proyecto al contenedor.
- RUN: instala las dependencias.
- EXPOSE: documenta el puerto que usa la app.
- CMD: define el comando que se ejecuta al iniciar el contenedor.

---

## 8. Construir la imagen manualmente

También puedes construir la imagen por separado:

```bash
docker build -t app-devops:1.0 .
```

Y luego ejecutarla:

```bash
docker run --rm -p 5000:5000 app-devops:1.0
```

---

## 9. Comandos básicos de Docker útiles

### Ver imágenes

```bash
docker images
```

### Ver contenedores en ejecución

```bash
docker ps
```

### Ver todos los contenedores

```bash
docker ps -a
```

### Ver logs del contenedor web

```bash
docker compose logs web
```

### Detener los servicios

```bash
docker compose down
```

### Entrar al contenedor

```bash
docker compose exec web bash
```

---

## 10. Probar Docker con una imagen oficial

Si quieres verificar que Docker funciona correctamente, puedes ejecutar:

```bash
docker run --rm hello-world
```

Esta imagen oficial sirve para comprobar que el motor de Docker está instalado y funcionando correctamente.

---

## 11. Archivo .dockerignore

Para mejorar el tiempo de construcción y evitar copiar archivos innecesarios, es recomendable crear un archivo .dockerignore con contenido como este:

```text
.git
__pycache__
*.pyc
*.log
.env
```

Esto ayuda a reducir el contexto de construcción y evita enviar archivos no necesarios al contenedor.

---

## 12. Buenas prácticas

- Mantener imágenes pequeñas y ligeras
- Usar variables de entorno para configuración sensible
- Etiquetar imágenes con versiones como 1.0, 1.1, etc.
- Usar Docker Compose para orquestar múltiples servicios
- No guardar credenciales directamente en el Dockerfile
- Limpiar contenedores y recursos no usados periódicamente

---

## 13. Problemas comunes

### Puerto ocupado
Si el puerto 5000 ya está en uso, cambia el mapeo en docker-compose.yml:

```yaml
ports:
  - "5001:5000"
```

### La base de datos aún no está lista
Es posible que la app intente conectarse antes de que PostgreSQL esté completamente listo. En ese caso, espera unos segundos y vuelve a recargar la página.

### Error al construir la imagen
Verifica que los archivos requirements.txt y setup.py estén correctos y que Docker tenga permisos para acceder al proyecto.

---

## 14. Recursos adicionales

- Documentación oficial de Docker: https://docs.docker.com
- Docker Hub: https://hub.docker.com
- Documentación de Docker Compose: https://docs.docker.com/compose/

---

## 15. Resumen rápido

Para levantar este proyecto con Docker:

```bash
docker compose up --build
```

Y para detenerlo:

```bash
docker compose down
```

Con esto tendrás la aplicación Flask y la base de datos PostgreSQL funcionando en contenedores de forma aislada y reproducible.
