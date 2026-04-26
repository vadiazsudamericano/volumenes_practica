# Informe de Práctica: Persistencia de Datos con Docker Volumes

## 1. Título
Análisis de volatilidad y configuración de persistencia mediante Docker Volumes y gestión de contenedores de bases de datos.

## 2. Tiempo de duración
60 minutos.

## 3. Fundamentos
La tecnología de contenedores permite aislar aplicaciones, pero por defecto, el almacenamiento en un contenedor es efímero; si el contenedor se elimina, los datos desaparecen. Docker resuelve esto mediante los **Volumes**, que son mecanismos para persistir datos generados y utilizados por contenedores. A diferencia de la capa de escritura del contenedor, los volúmenes residen en el sistema anfitrión, permitiendo que la información sobreviva a reinicios o eliminaciones de los contenedores.

En esta práctica, se utilizó un motor de base de datos para demostrar cómo un volumen vinculado a una ruta específica (en este caso `/data`) mantiene la integridad de la información. Esto es vital en el desarrollo de software profesional para asegurar que los datos de los usuarios no se pierdan ante fallos del sistema o actualizaciones de infraestructura.

## 4. Conocimientos previos
* **Gestión de imágenes y contenedores:** Comandos `docker run`, `docker rm` y `docker ps`.
* **Administración de Volúmenes:** Creación y vinculación de volúmenes mediante el flag `-v`.
* **Interacción con bases de datos:** Uso de CLI (Command Line Interface) para insertar y consultar registros.

## 5. Objetivos a alcanzar
* Demostrar la pérdida de datos en contenedores sin persistencia adecuada (Volatilidad).
* Implementar un volumen de Docker personalizado (`datos_valeria`) para el almacenamiento de registros.
* Validar la recuperación de información de la estudiante **Valeria Diaz** tras la recreación de un contenedor.

## 6. Equipo necesario
* Computador con conexión a internet.
* Entorno virtual de pruebas: **Killercoda Ubuntu Playground**.
* Docker versión 20.10.x o superior instalado.

## 7. Material de apoyo
* Documentación oficial de Docker sobre *Storage and Volumes*.
* Guía de la asignatura: Tendencias en Sistemas de Información.
* Referencia de comandos CLI para motores de bases de datos ligeros.

## 8. Procedimiento

Paso 1: Preparación del entorno y volumen
Se procedió a limpiar cualquier instancia previa y a crear el volumen dedicado para la persistencia de los datos.
```bash
docker rm -f servidor_valeria 2>/dev/null
docker volume create datos_valeria
Paso 2: Registro inicial de datos
Se ejecutó el contenedor vinculando el volumen y se registró la información de la estudiante.

Bash
docker run --name servidor_valeria -v datos_valeria:/data -d redis:alpine
docker exec -it servidor_valeria redis-cli set usuario "Valeria Diaz"
Paso 3: Demostración de Volatilidad (Falla de Persistencia)
Para validar la teoría, se eliminó el contenedor y se levantó uno nuevo sin vincular el volumen anterior. Al intentar recuperar el dato, el sistema devolvió un valor nulo.

Comando: docker exec -it servidor_valeria redis-cli get usuario

Resultado: (nil) (Confirmación de pérdida de datos).

Paso 4: Validación de Persistencia Real
Se levantó el contenedor vinculando correctamente el volumen datos_valeria. Al consultar nuevamente, la información se recuperó exitosamente.
## 9. Resultados esperados
Se logró demostrar satisfactoriamente el ciclo de vida de los datos en Docker. Las capturas de pantalla validan:

Estado Volátil: El retorno (nil) tras borrar el contenedor sin respaldo (Prueba de pérdida).

Estado Persistente: El retorno del registro "Valeria Diaz" tras vincular el volumen, demostrando que la información no depende del contenedor.

## 10. Bibliografía
Docker Inc. (2026). Manage data in Docker: Volumes. Recuperado de https://docs.docker.com/storage/volumes/

Redis, Inc. (2025). Persistence Documentation and Best Practices.
