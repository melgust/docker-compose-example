# Docker compose example

The following instructions are for creating the services in containers, namely the Backend, Frontend, and SQL Server 2022.

First, you must have a folder with the following structure.

```bash
project
    desaweb090
    desawebback
    docker-compose.yml
    demoapps.conf
```

desaweb090 is the frontend
desawebbacke is the backend

```bash
docker compose up --build
```

Connect via sqlcmd and run this script, change for your custom password

Get container name sql with

```bash
docker ps

```

In this exampe is: diario-sqlserver-1. Next step, get network name with:

```bash
docker inspect diario-sqlserver-1
```

And finaly connect with sqlcmd, change container name and network name

```bash
docker run -it --rm --network umg_appnet mcr.microsoft.com/mssql-tools /opt/mssql-tools/bin/sqlcmd -S umg-sqlserver-1 -U sa -P 'D3saweb.2025$'
```

```sql

CREATE DATABASE desaweb;
GO

USE desaweb;
GO

CREATE LOGIN desaweb WITH PASSWORD = 'D3saweb.2025';
CREATE USER desaweb FOR LOGIN desaweb;
EXEC sp_addrolemember 'db_owner', 'desaweb';
GO
```

Now start the backend

```bash
docker compose up -d
```

Connect to container via shell

```bash
docker exec -it diario-backend /bin/bash
```

Build again

```bash
docker compose up -d
```

Check containers

```bash
docker ps
```

Seeds table roles

```sql
INSERT INTO Roles (Name, CreatedAt, CreatedBy, UpdatedAt, UpdatedBy) VALUES(N'Admin', '2025-09-09 06:32:20.000', 0, '2025-09-09 06:32:40.000', 0);
INSERT INTO Roles (Name, CreatedAt, CreatedBy, UpdatedAt, UpdatedBy) VALUES(N'User', '2025-09-09 06:32:20.000', 0, '2025-09-09 06:32:40.000', 0);
INSERT INTO Roles (Name, CreatedAt, CreatedBy, UpdatedAt, UpdatedBy) VALUES(N'Guest', '2025-09-09 06:32:20.000', 0, '2025-09-09 06:32:40.000', 0);
```

Stop all containes

```bash
docker stop $(docker ps -a -q)
```
