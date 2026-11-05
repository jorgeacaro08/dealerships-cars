# Base de datos — Taller integrador: API de Concesionarios

Este repositorio **solo** contiene la base de datos PostgreSQL con datos de prueba.
El proyecto NestJS lo deben crear ustedes desde cero, en una carpeta aparte.

## Requisitos

- Docker Desktop (o Docker Engine + Docker Compose)

## Levantar la base de datos

```
docker compose up -d
```

La primera vez se crean las tablas `dealerships` y `cars` y se cargan los datos iniciales.

## Datos de conexión

| Dato       | Valor            |
|------------|------------------|
| Host       | localhost        |
| Puerto     | 5434             |
| Usuario    | dealer           |
| Contraseña | dealer123        |
| Base       | dealerships_db   |

(Se usa el puerto 5434 para no chocar con la base del taller de Marvel, que usa el 5433, ni con un PostgreSQL que ya tengan instalado.)

## Tablas

**dealerships**

| Columna     | Tipo         | Restricciones                 |
|-------------|--------------|-------------------------------|
| id          | SERIAL       | PRIMARY KEY                   |
| name        | VARCHAR(100) | NOT NULL, UNIQUE              |
| city        | VARCHAR(100) | NOT NULL                      |
| active      | BOOLEAN      | NOT NULL, DEFAULT TRUE        |
| created_at  | TIMESTAMP    | NOT NULL, DEFAULT NOW()       |
| updated_at  | TIMESTAMP    | NOT NULL, DEFAULT NOW()       |

**cars**

| Columna       | Tipo        | Restricciones                                          |
|---------------|-------------|--------------------------------------------------------|
| id            | SERIAL      | PRIMARY KEY                                            |
| plate         | VARCHAR(10) | NOT NULL, UNIQUE                                       |
| brand         | VARCHAR(50) | NOT NULL                                               |
| model         | VARCHAR(50) | NOT NULL                                               |
| year          | INTEGER     | NOT NULL, CHECK entre 1990 y 2027                      |
| price         | INTEGER     | NOT NULL, CHECK > 0 (pesos colombianos, sin decimales) |
| status        | VARCHAR(20) | NOT NULL, DEFAULT 'AVAILABLE', CHECK AVAILABLE / RESERVED / SOLD |
| dealership_id | INTEGER     | NOT NULL, FOREIGN KEY → dealerships(id) ON DELETE RESTRICT |
| created_at    | TIMESTAMP   | NOT NULL, DEFAULT NOW()                                |
| updated_at    | TIMESTAMP   | NOT NULL, DEFAULT NOW()                                |

## Comandos útiles

```
docker compose ps          # ver si está corriendo
docker compose logs -f     # ver logs
docker compose down        # apagar (los datos se conservan)
docker compose down -v     # apagar y BORRAR datos (al subir de nuevo se recargan los iniciales)
docker exec -it dealerships_db psql -U dealer -d dealerships_db   # consola SQL
```

## API

El proyecto NestJS esta en la carpeta `api/`, las instrucciones para correrlo y probarlo estan en `api/README.md`.
