# Oracle Express Edition 11g

Oracle XE 11g image build based on Oracle Database official images.

https://github.com/oracle/docker-images/tree/master/OracleDatabase/SingleInstance/dockerfiles/11.2.0.2

## Build image and run container

Oracle XE 11g installer file is splitted in directory *orainstaller*. Before building the image you must recompose the file:
```
./generateInstaller.sh
```

You can now build the image executing the following command:
```
docker build -t oracle-xe-11g .
```

To run a contanier:
```
docker run -d --name oracle-xe --shm-size=1g -p 1521:1521 -p 8080:8080 -e ORACLE_PWD=oracle oracle-xe-11g
````

This command will create a container running Oracle Express Edition 11g. You can customize the parameters:
- *name*: The name of the container.
- *shm-size*: Size of the shared memory. **Oracle XE requires a minimum of 1GB shared memory**.
- Port mapping:
    - *1521*: Listener port.
    - *8080*: Oracle Application Express web management console port.
- Environment variables:
    - *ORACLE_PWD*: sys and system users' password. If not specified, a random password will be generated. The password used is shown in Docker run logs, you can inspect by executing `docker logs oralce-xe`.
    - *SCHEMAS*: Schemas to be created when creating the container.

## Run container from Docker Hub

```
docker run -d --name oracle-xe --shm-size=1g -p 1521:1521 -p 8080:8080 -e ORACLE_PWD=oracle albmtez/oracle-xe-11g
```

## Schemas creation

Custom database schemas can be created on database initialization by specifying the names in the environment variable **SCHEMAS**. If no name is specified or the environment variable is not set, no schema will be created.

Example:
```
docker run -d --name oracle-xe --shm-size=1g -p 1521:1521 -p 8080:8080 -e ORACLE_PWD=oracle -SCHEMAS="EYSD KUSU KFUL" oracle-xe-11g
```

This command will create 3 database schemas with names EYSD, KUSU and KFUL, using as default tablespace *USERS* and as temporary space *TEMP*. The password is set using the name of the schema.

You can customize these values and the grants editing the file *template.schema.sql*.

## Data persisted on container recreation

## Custom scripts execution

## Database management

You can connect to the Oracle Application Express web management console with the following settings:
```
url: http://localhost:8080/apex
workspace: INTERNAL
user: ADMIN
password: oracle
```

Using sqlplus:
```
docker exec -it oracle-xe sqlplus sys/<ORACLE_PWD>@localhost:1521/XE as sysdba
```