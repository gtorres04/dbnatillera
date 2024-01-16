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


# LIQUIBASE
```
SheilaC.@MacBook-Pro-de-Gerlin dbnatillera % git config --global user.name "Sheila Cecilia Bermudez Florez"
SheilaC.@MacBook-Pro-de-Gerlin dbnatillera % git config --global user.email sheilabermudz@gmail.com
SheilaC.@MacBook-Pro-de-Gerlin dbnatillera % /Users/SheilaC./tools/liquibase-4.25.1/liquibase init project
####################################################
##   _     _             _ _                      ##
##  | |   (_)           (_) |                     ##
##  | |    _  __ _ _   _ _| |__   __ _ ___  ___   ##
##  | |   | |/ _` | | | | | '_ \ / _` / __|/ _ \  ##
##  | |___| | (_| | |_| | | |_) | (_| \__ \  __/  ##
##  \_____/_|\__, |\__,_|_|_.__/ \__,_|___/\___|  ##
##              | |                               ##
##              |_|                               ##
##                                                ## 
##  Get documentation at docs.liquibase.com       ##
##  Get certified courses at learn.liquibase.com  ## 
##                                                ##
####################################################
Starting Liquibase at 23:44:22 (version 4.25.1 #690 built at 2023-12-18 16:29+0000)
Liquibase Version: 4.25.1
Liquibase Open Source 4.25.1 by Liquibase
Setup new liquibase.properties, flowfile, and sample changelog? Enter (Y)es with defaults, yes with (C)ustomization, or (N)o. [Y]: 
y
Setting up new Liquibase project in '/Users/SheilaC./Documents/workspaces/natillera-wkps/dbnatillera/.'...

Created example changelog file '/Users/SheilaCDocuments/workspaces/natillera-wkps/dbnatillera/example-changelog.sql'
Created example defaults file '/Users/SheilaCDocuments/workspaces/natillera-wkps/dbnatillera/liquibase.properties'
Created example flow file '/Users/SheilaCDocuments/workspaces/natillera-wkps/dbnatillera/liquibase.advanced.flowfile.yaml'
Created example flow file '/Users/SheilaCDocuments/workspaces/natillera-wkps/dbnatillera/liquibase.flowvariables.yaml'
Created example flow file '/Users/SheilaCDocuments/workspaces/natillera-wkps/dbnatillera/liquibase.endstage.flow'
Created example flow file '/Users/SheilaCDocuments/workspaces/natillera-wkps/dbnatillera/liquibase.flowfile.yaml'
Created example checks package '/Users/SheilaCDocuments/workspaces/natillera-wkps/dbnatillera/liquibase.checks-package.yaml'

To use the new project files make sure your database is active and accessible by opening a new terminal window to run "liquibase init start-h2", and then return to this terminal window to run "liquibase update" command.
For more details, visit the Getting Started Guide at https://docs.liquibase.com/start/home.html
Liquibase command 'init project' was executed successfully.
SheilaC.@MacBook-Pro-de-Gerlin dbnatillera % docker run --name liquibase-postgres -e POSTGRES_PASSWORD=postgres -d postgres
Unable to find image 'postgres:latest' locally
latest: Pulling from library/postgres
Digest: sha256:49c276fa02e3d61bd9b8db81dfb4784fe814f50f778dce5980a03817438293e3
Status: Downloaded newer image for postgres:latest
82d7f13080eb09ff261c6123e4d7dac612e1931ac68843f9fcf7eaec54cf3f4d
SheilaC.@MacBook-Pro-de-Gerlin dbnatillera % docker run --name liquibase-postgres -p 5454:5432 -e POSTGRES_PASSWORD=postgres -d postgres
88bb6a7066c288b53aa13d698e5213b21fae9def4a2ec9f92578543f1f500219
SheilaC.@MacBook-Pro-de-Gerlin dbnatillera % cd liquibase-project 
SheilaC.@MacBook-Pro-de-Gerlin liquibase-project % /Users/SheilaC./tools/liquibase-4.25.1/liquibase update                                    
####################################################
##   _     _             _ _                      ##
##  | |   (_)           (_) |                     ##
##  | |    _  __ _ _   _ _| |__   __ _ ___  ___   ##
##  | |   | |/ _` | | | | | '_ \ / _` / __|/ _ \  ##
##  | |___| | (_| | |_| | | |_) | (_| \__ \  __/  ##
##  \_____/_|\__, |\__,_|_|_.__/ \__,_|___/\___|  ##
##              | |                               ##
##              |_|                               ##
##                                                ## 
##  Get documentation at docs.liquibase.com       ##
##  Get certified courses at learn.liquibase.com  ## 
##                                                ##
####################################################
Starting Liquibase at 00:07:55 (version 4.25.1 #690 built at 2023-12-18 16:29+0000)
Liquibase Version: 4.25.1
Liquibase Open Source 4.25.1 by Liquibase
Running Changeset: example-changelog.sql::1::your.name

UPDATE SUMMARY
Run:                          3
Previously run:               0
Filtered out:                 0
-------------------------------
Total change sets:            3

ERROR: Exception Details
ERROR: Exception Primary Class:  PSQLException
ERROR: Exception Primary Reason: ERROR: syntax error at or near "auto_increment"
  Position: 46
ERROR: Exception Primary Source: PostgreSQL 16.1 (Debian 16.1-1.pgdg120+1)

Unexpected error running Liquibase: Migration failed for changeset example-changelog.sql::1::your.name:
     Reason: liquibase.exception.DatabaseException: ERROR: syntax error at or near "auto_increment"
  Position: 46 [Failed SQL: (0) create table person (
    id int primary key auto_increment not null,
    name varchar(50) not null,
    address1 varchar(50),
    address2 varchar(50),
    city varchar(30)
)]

For more information, please use the --log-level flag
SheilaC.@MacBook-Pro-de-Gerlin liquibase-project % /Users/SheilaC./tools/liquibase-4.25.1/liquibase update
####################################################
##   _     _             _ _                      ##
##  | |   (_)           (_) |                     ##
##  | |    _  __ _ _   _ _| |__   __ _ ___  ___   ##
##  | |   | |/ _` | | | | | '_ \ / _` / __|/ _ \  ##
##  | |___| | (_| | |_| | | |_) | (_| \__ \  __/  ##
##  \_____/_|\__, |\__,_|_|_.__/ \__,_|___/\___|  ##
##              | |                               ##
##              |_|                               ##
##                                                ## 
##  Get documentation at docs.liquibase.com       ##
##  Get certified courses at learn.liquibase.com  ## 
##                                                ##
####################################################
Starting Liquibase at 00:10:42 (version 4.25.1 #690 built at 2023-12-18 16:29+0000)
Liquibase Version: 4.25.1
Liquibase Open Source 4.25.1 by Liquibase
Running Changeset: example-changelog.sql::1::your.name

UPDATE SUMMARY
Run:                          3
Previously run:               0
Filtered out:                 0
-------------------------------
Total change sets:            3

ERROR: Exception Details
ERROR: Exception Primary Class:  PSQLException
ERROR: Exception Primary Reason: ERROR: syntax error at or near "auto_increment"
  Position: 46
ERROR: Exception Primary Source: PostgreSQL 16.1 (Debian 16.1-1.pgdg120+1)

Unexpected error running Liquibase: Migration failed for changeset example-changelog.sql::1::your.name:
     Reason: liquibase.exception.DatabaseException: ERROR: syntax error at or near "auto_increment"
  Position: 46 [Failed SQL: (0) create table person (
    id int primary key auto_increment not null,
    name varchar(50) not null,
    address1 varchar(50),
    address2 varchar(50),
    city varchar(30)
)]

For more information, please use the --log-level flag
SheilaC.@MacBook-Pro-de-Gerlin liquibase-project % /Users/SheilaC./tools/liquibase-4.25.1/liquibase update
####################################################
##   _     _             _ _                      ##
##  | |   (_)           (_) |                     ##
##  | |    _  __ _ _   _ _| |__   __ _ ___  ___   ##
##  | |   | |/ _` | | | | | '_ \ / _` / __|/ _ \  ##
##  | |___| | (_| | |_| | | |_) | (_| \__ \  __/  ##
##  \_____/_|\__, |\__,_|_|_.__/ \__,_|___/\___|  ##
##              | |                               ##
##              |_|                               ##
##                                                ## 
##  Get documentation at docs.liquibase.com       ##
##  Get certified courses at learn.liquibase.com  ## 
##                                                ##
####################################################
Starting Liquibase at 00:10:53 (version 4.25.1 #690 built at 2023-12-18 16:29+0000)
Liquibase Version: 4.25.1
Liquibase Open Source 4.25.1 by Liquibase
Running Changeset: example-changelog.sql::1::your.name
Running Changeset: example-changelog.sql::2::your.name
Running Changeset: example-changelog.sql::3::other.dev

UPDATE SUMMARY
Run:                          3
Previously run:               0
Filtered out:                 0
-------------------------------
Total change sets:            3

Liquibase: Update has been successful. Rows affected: 3
Liquibase command 'update' was executed successfully.
SheilaC.@MacBook-Pro-de-Gerlin liquibase-project % /Users/SheilaC./tools/liquibase-4.25.1/liquibase update
```
