# Database Setup
These site collects information's about how to combine application server with databases.


* [JBoss and Postgresql](#jboss-and-postgresql)
* [JBoss and MYSQL](#jboss-and-mysql)
* [JBoss and Oracle](#jboss-and-oracle)
* [Wildfly and Postgresql](#wildfly-and-postgresql)
* [Wildfly and Oracle](#wildfly-and-oracle)
* [Wildfly and MariaDB](#wildfly-and-mariadb)
* [Tomcat and Postgresql](#tomcat-and-postgresql)
* [Tomcat and MSSQL](#tomcat-and-mssql)
* [Tomcat and Oracle](#tomcat-and-oracle)


# JBoss and Postgresql

## standalone.xml

Location: ```%SERVER_HOME/standalone/configuration/standalone.xml```  
```
		<datasources>
		    <datasource jndi-name="java:jboss/datasources/ExampleDS" pool-name="ExampleDS" enabled="true" use-java-context="true">
		        <connection-url>jdbc:h2:mem:test;DB_CLOSE_DELAY=-1;DB_CLOSE_ON_EXIT=FALSE</connection-url>
		        <driver>h2</driver>
		        <security>
		            <user-name>sa</user-name>
		            <password>sa</password>
		        </security>
		    </datasource>
		    <datasource jta="true" jndi-name="java:jboss/datasources/ProcessEngine" pool-name="ProcessEngine" enabled="true" use-java-context="true" use-ccm="true">
		        <connection-url>jdbc:postgresql://portainer.camunda.loc:30066/process-engine</connection-url>
		        <driver>postgresql</driver>
		        <security>
		            <user-name>camunda</user-name>
		            <password>camunda</password>
		        </security>
		    </datasource>
		    <drivers>
		        <driver name="h2" module="com.h2database.h2">
		            <xa-datasource-class>org.h2.jdbcx.JdbcDataSource</xa-datasource-class>
		        </driver>
		        <driver name="postgresql" module="org.postgresql">
		            <xa-datasource-class>org.postgresql.ds.PGSimpleDataSource</xa-datasource-class>
		        </driver>
		    </drivers>
		</datasources>
```

## jdbc driver
Download: https://jdbc.postgresql.org/download.html  
Location: copy the driver jar to ```%SERVER_HOME/modules/org/postgresql/main```  
The following driver are compatible to your JDK version:
|  Java 7  |  Java 8 (and higher)  |
|----------|:---------------------:|
| JDBC 4.1 | JDBC 4.2              |


## module.xml
create the file module.xml in ```%SERVER_HOME/modules/org/postgresql/main``` with the following content. Check if resource root path matches the version of the downloaded driver jar.

```
<?xml version="1.0" encoding="UTF-8"?>

<module xmlns="urn:jboss:module:1.1" name="org.postgresql">

    <resources>
        <resource-root path="postgresql-42.2.14.jre7.jar"/>
        <!-- Insert resources here -->
    </resources>
    <dependencies>
        <module name="javax.api"/>
        <module name="javax.transaction.api"/>
        <module name="javax.servlet.api" optional="true"/>
    </dependencies>
</module>

```

# JBoss and MYSQL

## standalone.xml

Location: ```%SERVER_HOME/standalone/configuration/standalone.xml```  

```
<datasources>
    <datasource jndi-name="java:jboss/datasources/ExampleDS" pool-name="ExampleDS" enabled="true" use-java-context="true">
        <connection-url>jdbc:h2:mem:test;DB_CLOSE_DELAY=-1</connection-url>
        <driver>h2</driver>
        <security>
            <user-name>sa</user-name>
            <password>sa</password>
        </security>
    </datasource>
    <datasource jta="true" jndi-name="java:jboss/datasources/ProcessEngine" pool-name="ProcessEngine" enabled="true" use-java-context="true" use-ccm="true">
        <connection-url>jdbc:mysql://localhost:3306/process-engine</connection-url>
        <driver>mysql</driver>
        <security>
            <user-name>root</user-name>
            <password></password>
        </security>                   
    </datasource>               
    <datasource jta="true" jndi-name="java:jboss/datasources/CycleDS" pool-name="CycleDS" enabled="true" use-java-context="true" use-ccm="true">
        <connection-url>jdbc:mysql://localhost:3306/cycle</connection-url>
        <driver>mysql</driver>
        <pool></pool>
        <security>
            <user-name>root</user-name>
            <password></password>
        </security>
    </datasource>
    <drivers>
        <driver name="h2" module="com.h2database.h2">
            <xa-datasource-class>org.h2.jdbcx.JdbcDataSource</xa-datasource-class>
        </driver>
        <driver name="mysql" module="com.mysql">
            <xa-datasource-class>com.mysql.jdbc.jdbc2.optional.MysqlXADataSource</xa-datasource-class>
        </driver>
    </drivers>
</datasources>
```

## jdbc driver
copy the driver jar to ```%SERVER_HOME/modules/com/mysql/main```

## module.xml
create the file module.xml in ```%SERVER_HOME/modules/com/mysql/main``` with the following content. Check if resource root path matches the version of the downloaded driver jar.

```
<?xml version="1.0" encoding="UTF-8"?>

<module xmlns="urn:jboss:module:1.1" name="com.mysql">

    <resources>
        <resource-root path="mysql-connector-java-5.1.28-bin.jar"/>
        <!-- Insert resources here -->
    </resources>
    <dependencies>
        <module name="javax.api"/>
        <module name="javax.transaction.api"/>
        <module name="javax.servlet.api" optional="true"/>
    </dependencies>
</module>

```

# JBoss and ORACLE

## standalone.xml

Location: ```%SERVER_HOME/standalone/configuration/standalone.xml```  

```

<datasources>
	<datasource jndi-name="java:jboss/datasources/ExampleDS" pool-name="ExampleDS" enabled="true" use-java-context="true">
	<connection-url>jdbc:h2:mem:test;DB_CLOSE_DELAY=-1;DB_CLOSE_ON_EXIT=FALSE</connection-url>
	<driver>h2</driver>
	<security>
	    <user-name>sa</user-name>
	    <password>sa</password>
	</security>
    </datasource>
    <datasource jndi-name="java:jboss/datasources/ProcessEngine" pool-name="ProcessEngine" enabled="true" use-java-context="true">
	<connection-url>jdbc:oracle:thin:@portainer.camunda.loc:30087:XE</connection-url>
	<driver>oracle</driver>
	<security>
	    <user-name>camunda</user-name>
	    <password>camunda</password>
	</security>
    </datasource>
    <drivers>
	 <driver name="h2" module="com.h2database.h2">
	    <xa-datasource-class>org.h2.jdbcx.JdbcDataSource</xa-datasource-class>
    </driver>
	<driver name="oracle" module="com.oracle">
	    <driver-class>oracle.jdbc.driver.OracleDriver</driver-class>
	</driver>
    </drivers>
</datasources>

```

## jdbc driver
copy the driver jar to ```%SERVER_HOME/modules/com/oracle/main```

## module.xml
create the file module.xml in ```%SERVER_HOME/modules/com/oracle/main``` with the following content. Check if resource root path matches the version of the downloaded driver jar.

```
<?xml version="1.0" encoding="UTF-8"?>

<module xmlns="urn:jboss:module:1.1" name="com.oracle">

    <resources>
        <resource-root path="ojdbc8.jar"/>
        <!-- Insert resources here -->
    </resources>
    <dependencies>
        <module name="javax.api"/>
        <module name="javax.transaction.api"/>
        <module name="javax.servlet.api" optional="true"/>
    </dependencies>
</module>

```

# Wildfly and Postgresql

## standalone.xml

Location: ```%SERVER_HOME/standalone/configuration/standalone.xml```  

```
<datasources>
    <datasource jndi-name="java:jboss/datasources/ExampleDS" pool-name="ExampleDS" enabled="true" use-java-context="true">
        <connection-url>jdbc:h2:mem:test;DB_CLOSE_DELAY=-1;DB_CLOSE_ON_EXIT=FALSE</connection-url>
        <driver>h2</driver>
        <security>
            <user-name>sa</user-name>
            <password>sa</password>
        </security>
    </datasource>
    <datasource jta="true" jndi-name="java:jboss/datasources/ProcessEngine" pool-name="ProcessEngine" enabled="true" use-java-context="true" use-ccm="true">
        <connection-url>jdbc:postgresql://localhost:5432/process-engine</connection-url>
        <driver>postgresql</driver>
        <security>
            <user-name>postgres</user-name>
            <password>cam123</password>
        </security>
    </datasource>
    <drivers>
        <driver name="h2" module="com.h2database.h2">
            <xa-datasource-class>org.h2.jdbcx.JdbcDataSource</xa-datasource-class>
        </driver>
        <driver name="postgresql" module="org.postgresql">
            <xa-datasource-class>org.postgresql.ds.PGSimpleDataSource</xa-datasource-class>
        </driver>
    </drivers>
</datasources>
```


## jdbc driver
download a proper jdbc drive
copy the driver jar to ```%SERVER_HOME/modules/org/postgresql/main```

## module.xml
create the file module.xml in ```%SERVER_HOME/modules/org/postgresql/main``` with the following content. Check if resource root path matches the version of the downloaded driver jar.

```
<?xml version="1.0" encoding="UTF-8"?>

<module xmlns="urn:jboss:module:1.1" name="org.postgresql">

    <resources>
        <resource-root path="postgresql-9.3-1102.jdbc4.jar"/>
        <!-- Insert resources here -->
    </resources>
    <dependencies>
        <module name="javax.api"/>
        <module name="javax.transaction.api"/>
        <module name="javax.servlet.api" optional="true"/>
    </dependencies>
</module>


```

# Wildfly and Oracle

## standalone.xml

Location: ```%SERVER_HOME/standalone/configuration/standalone.xml```  

```
<datasources>
    <datasource jndi-name="java:jboss/datasources/ExampleDS" pool-name="ExampleDS" enabled="true" use-java-context="true">
        <connection-url>jdbc:h2:mem:test;DB_CLOSE_DELAY=-1;DB_CLOSE_ON_EXIT=FALSE</connection-url>
        <driver>h2</driver>
        <security>
            <user-name>sa</user-name>
            <password>sa</password>
        </security>
    </datasource>
    <datasource jndi-name="java:jboss/datasources/ProcessEngine" pool-name="ProcessEngine" enabled="true" use-java-context="true" jta="true" use-ccm="true">
        <connection-url>jdbc:oracle:thin:@192.168.178.102:1521/XE</connection-url>
        <driver>oracle</driver>
        <security>
            <user-name>camunda</user-name>
            <password>camunda</password>
        </security>
    </datasource>
    <drivers>
        <driver name="h2" module="com.h2database.h2">
            <xa-datasource-class>org.h2.jdbcx.JdbcDataSource</xa-datasource-class>
        </driver>
        <driver name="oracle" module="com.oracle">
            <xa-datasource-class>oracle.jdbc.xa.client.OracleXADataSource</xa-datasource-class>
        </driver>
    </drivers>
</datasources>
```


## jdbc driver
copy the driver jar to ```%SERVER_HOME/modules/com/oracle/main```

## module.xml
create the file module.xml in ```%SERVER_HOME/modules/com/oracle/main``` with the following content. Check if resource root path matches the version of the downloaded driver jar.

```
<?xml version="1.0" encoding="UTF-8"?>

<module xmlns="urn:jboss:module:1.3" name="com.oracle">

    <resources>
        <resource-root path="ojdbc6-11.2.0.1.0.jar"/>
    </resources>
    <dependencies>
        <module name="javax.api"/>
        <module name="javax.transaction.api"/>
        <module name="javax.servlet.api" optional="true"/>
    </dependencies>
</module>
```


# WildFly and MariaDB

Create new module folder structure and add a module.xml
```
$ mkdir -p server/wildfly-10.1.0.Final/modules/org/mariadb/jdbc/mariadb-java-client/main/
$ vim server/wildfly-10.1.0.Final/modules/org/mariadb/jdbc/mariadb-java-client/main/module.xml
```

The module xml should contain the following:
```xml
<module xmlns="urn:jboss:module:1.1" name="org.mariadb.jdbc.mariadb-java-client">
  <resources>
    <resource-root path="mariadb-java-client-@version.mariadb@.jar"/>
  </resources>

  <dependencies>
    <module name="javax.api"/>
    <module name="javax.transaction.api" />
  </dependencies>
</module>
```

Copy the jdbc driver into the folder in which the `module.xml` lies.
Maybe the jdbc driver is already in the `.m2` folder.
```
$ cp ~/.m2/repository/org/mariadb/jdbc/mariadb-java-client/1.1.8/mariadb-java-client-1.1.8.jar server/wildfly-10.1.0.Final/modules/org/mariadb/jdbc/mariadb-java-client/main/
```

Adjust the copied module xml. Add the current jdbc mariadb version.
```
$ vim server/wildfly-10.1.0.Final/modules/org/mariadb/jdbc/mariadb-java-client/main/module.xml
```


Adjust the `standalone.xml`, add the driver tag and adjust the data source.
The data source should contain the url, username and password which is needed.
```
$ vim server/wildfly-10.1.0.Final/standalone/configuration/standalone.xml 
```

```
   <datasources>
     <datasource jndi-name="java:jboss/datasources/ExampleDS" pool-name="ExampleDS" enabled="true" use-java-context="true">
       <connection-url>jdbc:h2:mem:test;DB_CLOSE_DELAY=-1;DB_CLOSE_ON_EXIT=FALSE</connection-url>
         <driver>h2</driver>
         <security>
           <user-name>sa</user-name>
           <password>sa</password>
         </security>
       </datasource>
       <datasource jndi-name="java:jboss/datasources/ProcessEngine" pool-name="ProcessEngine" enabled="true" use-java-context="true" jta="true" use-ccm="true">
         <connection-url>jdbc:mariadb://localhost:3306/process-engine</connection-url>
           <driver>org.mariadb</driver>
             <security>
               <user-name>camunda</user-name>
               <password>camunda</password>
            </security>
       </datasource>
     <drivers>
       <driver name="h2" module="com.h2database.h2">
         <xa-datasource-class>org.h2.jdbcx.JdbcDataSource</xa-datasource-class>
       </driver>
       <driver name="org.mariadb" module="org.mariadb.jdbc.mariadb-java-client">
         <xa-datasource-class>org.mariadb.jdbc.MySQLDataSource</xa-datasource-class>
       </driver>
     </drivers>
   </datasources>         
```
If you haven't already setup the mariadb. Pull the mariadb image with docker.
After that start the mariadb, also with docker.
```
docker pull registry.camunda.com/camunda-ci-mariadb
docker run -d -p 3306:3306 registry.camunda.com/camunda-ci-mariadb
```  

# Tomcat and Postgresql

## server.xml  

Location: ```%SERVER_HOME/conf/server.xml```

```
<Resource name="jdbc/ProcessEngine"
              auth="Container"
              type="javax.sql.DataSource"
              factory="org.apache.tomcat.jdbc.pool.DataSourceFactory"
              uniqueResourceName="process-engine"
              driverClassName="org.postgresql.Driver"
              url="jdbc:postgresql://192.168.0.11:30172/process-engine"
              defaultTransactionIsolation="READ_COMMITTED"
              username="camunda"
              password="camunda"
              maxActive="20"
              minIdle="5" />
```


## jdbc driver  

Download: https://jdbc.postgresql.org/download.html
copy the jdbc driver to ```%SERVER_HOME/lib```


# Tomcat and MSSQL

## server.xml  

Location: ```%SERVER_HOME/conf/server.xml```

```<Resource name="jdbc/ProcessEngine"
              auth="Container"
              type="javax.sql.DataSource" 
              factory="org.apache.tomcat.jdbc.pool.DataSourceFactory"
              uniqueResourceName="process-engine"
              driverClassName="com.microsoft.sqlserver.jdbc.SQLServerDriver" 
              url="jdbc:sqlserver://portainer.camunda.loc:30352;DatabaseName=master"
              username="SA"  
              password="camunda_123"  
              maxPoolSize="20"  
              minPoolSize="5" />
```


## jdbc driver  
download Microsoft JDBC Driver for SQL Server and copy mssql-jdbc-8.2.2.jre8.jar to ```%SERVER_HOME/lib```
https://docs.microsoft.com/en-us/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server?view=sql-server-ver15

# Tomcat and Oracle

## server.xml  

Location: ```%SERVER_HOME/conf/server.xml```

```
    <<Resource name="jdbc/ProcessEngine"
              auth="Container"
              type="javax.sql.DataSource"
              factory="org.apache.tomcat.jdbc.pool.DataSourceFactory"
              uniqueResourceName="process-engine"
              driverClassName="oracle.jdbc.OracleDriver"
              url="jdbc:oracle:thin:@localhost:1521/xe"
              defaultTransactionIsolation="READ_COMMITTED"
              username="camunda"
              password="cam123"
              maxActive="20"
              minIdle="5" /> 
```


## jdbc driver  

Download a proper jdbc driver (e.g. ojdbc7.jar)
copy the jdbc driver to ```%SERVER_HOME/lib```

