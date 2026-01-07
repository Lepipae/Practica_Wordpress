# Guía de Uso: Docker Compose WordPress

## Descripción General

Este archivo `docker-compose.yml` configura un entorno completo de WordPress con MySQL y phpMyAdmin, utilizando contenedores Docker y una red compartida.

## Servicios Incluidos

### WordPress
- **Imagen**: wordpress:latest
- **Puerto**: 80
- **Volumen**: `/var/www/html` (wordpress_data)
- **Propósito**: Servidor web con WordPress

### MySQL
- **Imagen**: mysql:8.0
- **Volumen**: `/var/lib/mysql` (mysql_data)
- **Propósito**: Base de datos para WordPress

### phpMyAdmin
- **Imagen**: phpmyadmin:latest
- **Puerto**: 8080
- **Propósito**: Interfaz web para gestionar la base de datos

## Configuración Requerida

Asegúrate de que el archivo `.env` existe en el mismo directorio que `docker-compose.yml` con las siguientes variables:

```
WORDPRESS_DB_HOST=mysql
WORDPRESS_DB_USER=wordpress
WORDPRESS_DB_PASSWORD="tu_contraseña"
WORDPRESS_DB_NAME=wordpress
MYSQL_ROOT_PASSWORD="tu_contraseña"
MYSQL_DATABASE=wordpress
MYSQL_USER=wordpress
MYSQL_PASSWORD="tu_contraseña"
PMA_HOST=mysql
PMA_USER=wordpress
PMA_PASSWORD="tu_contraseña"
```

## Comandos Principales

### Iniciar los contenedores
```bash
docker-compose up -d
```

### Detener los contenedores
```bash
docker-compose down
```

### Ver logs en tiempo real
```bash
docker-compose logs -f
```

### Reconstruir las imágenes
```bash
docker-compose up -d --build
```

### Listar contenedores activos
```bash
docker-compose ps
```

## Acceso a los Servicios

- **WordPress**: http://localhost:80
- **phpMyAdmin**: http://localhost:8080

## Volúmenes

Los datos se persisten en dos volúmenes:
- `wordpress_data`: Contenido de WordPress
- `mysql_data`: Base de datos MySQL

Estos volúmenes se crean automáticamente al ejecutar `docker-compose up`.

## Red

Los servicios se comunican a través de la red `wordpress_network` (tipo bridge). Esta red facilita la comunicación entre contenedores mediante nombres de servicio.

## Dependencias

- WordPress depende de MySQL
- phpMyAdmin depende de MySQL

El archivo está configurado para iniciar primero MySQL, seguido de los otros servicios.

## Solución de Problemas

### Los contenedores no inician
- Verifica que el puerto 80 y 8080 no estén en uso
- Revisa que el archivo `.env` esté en el directorio correcto
- Ejecuta `docker-compose logs` para ver mensajes de error

### Conexión rechazada a la base de datos
- Asegúrate de que las credenciales en `.env` coincidan con las configuradas en docker-compose.yml
- Verifica que el contenedor MySQL haya iniciado correctamente

### Base de datos no persiste
- Confirma que los volúmenes existen: `docker volume ls`
- No elimines los volúmenes con `docker-compose down -v` si deseas conservar los datos
