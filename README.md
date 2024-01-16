COMPOSE_PROFILES=pre docker compose -p natillera up -d db
docker compose logs -f db
docker compose down --volumes
docker volume ls

Lenguaje de Consulta de Datos (DQL) - El Lenguaje de Consulta de Datos es el sublenguaje responsable de leer, o consultar, datos de una base de datos. En SQL, corresponde al SELECT
Lenguaje deManipulación de Datos (DML ) - El Lenguaje de Manipulación de Datos es el sublenguaje responsable de añadir, editar o borrar datos de una base de datos. En SQL, corresponde a los lenguajes INSERT, UPDATE, y DELETE
Lenguaje de Definición de Datos (DDL ) - El Lenguaje de Definición de Datos es el sublenguaje responsable de definir la forma en que se estructuran los datos en una base de datos. En SQL, esto corresponde a la manipulación de tablas a través de CREATE TABLE, ALTER TABLE, y DROP TABLE
Lenguaje de Control de Datos (DCL) - El Lenguaje de Control de Datos es el sublenguaje responsable de las tareas administrativas de control de la propia base de datos, especialmente la concesión y revocación de permisos de base de datos para los usuarios. En SQL, esto corresponde a los comandos GRANT, REVOKE, y DENY, entre otros.


CREATE SCHEMA IF NOT EXISTS natillera;

CREATE ROLE dql;
CREATE ROLE dml;
CREATE ROLE ddl;
CREATE ROLE dcl;

GRANT USAGE ON SCHEMA natillera TO dql;
GRANT USAGE ON SCHEMA natillera TO dml;
GRANT CREATE ON SCHEMA natillera TO ddl;
GRANT CREATE ON DATABASE natillera TO dcl;

ALTER DEFAULT PRIVILEGES IN SCHEMA natillera GRANT SELECT ON TABLES TO dql;
ALTER DEFAULT PRIVILEGES IN SCHEMA natillera GRANT INSERT, UPDATE, DELETE ON TABLES TO dml;

CREATE USER backoffice WITH PASSWORD 'backoffice';
GRANT dql TO backoffice;
GRANT dml TO backoffice;

CREATE USER analyst WITH PASSWORD 'analyst';
GRANT dql TO analyst;
GRANT dml TO analyst;
GRANT ddl TO analyst;

CREATE USER dba WITH PASSWORD 'dba';
GRANT dql TO dba;
GRANT dml TO dba;
GRANT ddl TO dba;
GRANT dcl TO dba;

--Podemos utilizar la siguiente consulta para obtener 
--una lista de todos los usuarios y roles de la base 
--de datos junto con una lista de roles que se les han concedido:

SELECT
r.rolname,
ARRAY(SELECT b.rolname
FROM pg_catalog.pg_auth_members m
JOIN pg_catalog.pg_roles b ON (m.roleid = b.oid)
WHERE m.member = r.oid) as memberof
FROM pg_catalog.pg_roles r
WHERE r.rolname NOT IN ('pg_signal_backend','rds_iam',
'rds_replication','rds_superuser',
'rdsadmin','rdsrepladmin')
ORDER BY 1;
