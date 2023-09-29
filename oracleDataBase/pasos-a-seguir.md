# Instrucciones para utilizar Oracle Database en Docker

A continuación, se presentan los pasos para configurar y ejecutar una instancia de Oracle Database en Docker utilizando el Oracle Container Registry.

## 1. Crear una cuenta en Oracle Container Registry

Primero, debes crear una cuenta en Oracle Container Registry si aún no tienes una. Puedes registrarte en [Oracle Container Registry](https://container-registry.oracle.com) utilizando tu información de inicio de sesión de Oracle.

## 2. Iniciar sesión desde Docker

Luego de crear tu cuenta en Oracle Container Registry, inicia sesión desde Docker utilizando el siguiente comando:

```bash
docker login container-registry.oracle.com
```
Ingresa tus credenciales de Oracle para completar el proceso de inicio de sesión.

## 3. Descargar la imagen Docker de Oracle Database
Descarga la imagen Docker de Oracle Database ejecutando el siguiente comando:
```bash
docker pull container-registry.oracle.com/database/enterprise:latest
```
Asegúrate de tener una conexión a Internet activa, ya que esta imagen puede ser bastante grande y tomará tiempo descargarla.

## 4. Ejecutar el contenedor Docker de Oracle Database
Una vez que hayas descargado la imagen, puedes ejecutar un contenedor Docker de Oracle Database con la siguiente instrucción:

```bash
docker run -d -it --name oracle-db -p 1521:1521 -p 5500:5500 -e ORACLE_SID=ORCLCDB -e ORACLE_PDB=ORCLPDB1 -e ORACLE_PWD=1234 container-registry.oracle.com/database/enterprise:latest
```
* -d: Ejecuta el contenedor en segundo plano.
* -it: Habilita la interacción con el contenedor.
* --name oracle-db: Asigna un nombre al contenedor (en este caso, "oracle-db").
* -p 1521:1521 -p 5500:5500: Mapea los puertos del host a los puertos del contenedor para permitir el acceso a la base de datos.
* -e ORACLE_SID=ORCLCDB -e ORACLE_PDB=ORCLPDB1 -e ORACLE_PWD=1234: Configura variables de entorno para la instancia de Oracle Database.
El contenedor se ejecutará con la configuración proporcionada.

Recuerda que esta es una configuración básica y que debes personalizarla según tus necesidades específicas.

¡Listo! Ahora tienes una instancia de Oracle Database en ejecución en un contenedor Docker. Puedes conectarte a la base de datos utilizando las herramientas adecuadas y comenzar a trabajar con ella.

> **Nota:** Asegúrate de seguir las mejores prácticas de seguridad al configurar y utilizar tu base de datos Oracle en Docker.

## 5. Conexión a la base de datos
Para conectarte con la configuración anterior deberías utilizar los siguientes datos:
* Host: localhost
* Port: 1521
* Database: ORCLPDB1 (service name)
* Autenticatión:
  * Autenticación: Oracle database native
  * Username: system
  * Role: Normal
  * Password: 1234


## Otros datos
* Server: Oracle Oracle Database 21c Enterprise Edition Release 21.0.0.0.0 - Production
* Driver: Oracle JDBC driver 12.2.0.1.0
* Herramienta de administración de bases de datos con la que se probó: DBeaver
* [Documentación utilizada](https://container-registry.oracle.com/ords/f?p=113:4:120693202410983:::4:P4_REPOSITORY,AI_REPOSITORY,AI_REPOSITORY_NAME,P4_REPOSITORY_NAME,P4_EULA_ID,P4_BUSINESS_AREA_ID:9,9,Oracle%20Database%20Enterprise%20Edition,Oracle%20Database%20Enterprise%20Edition,1,0&cs=3CurpWdm1xAd5o43qsVtWiSuVvsJakXeESb0l6glWeA3vR7LKLwqsN6TbblvZcvCGbo36OSsNE7rNu8zTAJVXLw)

### Generación de usuario con privilegios en BBDD
````sql
CREATE USER srv_expedientes IDENTIFIED BY 1234; --Crea usuario con password
GRANT DBA TO srv_expedientes; --Asigna permisos
````

### Configuración en el application.yml de un proyecto Java

````yml
spring:
  datasource:
    url: jdbc:oracle:thin:@localhost:1521/ORCLPDB1 #dependiendo de la configuracion utilizada puede variar
    username: srv_expedientes #usuario configurado
    password: 1234 #password configurada
    driver-class-name: oracle.jdbc.OracleDriver
  jpa:
    hibernate:
      ddl-auto: update
      show-sql: true
      properties:
        hibernate:
        format_sql: true
        dialect: org.hibernate.dialect.Oracle10gDialect
````

### Dependencias Maven necesarias
````xml
    <!-- JPA -->
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-jpa</artifactId>
      <version>3.1.2</version>
    </dependency>
    <!-- Driver Oracle-->
    <dependency>
      <groupId>com.oracle.database.jdbc</groupId>
      <artifactId>ojdbc10</artifactId>
      <version>19.20.0.0</version>
    </dependency>
````

