# JBoss and Postgresql

## platform.xml
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
        <connection-url>jdbc:postgresql://192.168.178.109:5433/process-engine</connection-url>
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
copy the driver jar to ```%SERVER_HOME\modules\org\postgresql\main```

## module.xml
create the file module.xml in ```%SERVER_HOME\modules\org\postgresql\main``` with the following content. Check if resource root path matches the version of the downloaded driver jar.

```
<?xml version="1.0" encoding="UTF-8"?>

<!--
  ~ JBoss, Home of Professional Open Source.
  ~ Copyright 2010, Red Hat, Inc., and individual contributors
  ~ as indicated by the @author tags. See the copyright.txt file in the
  ~ distribution for a full listing of individual contributors.
  ~
  ~ This is free software; you can redistribute it and/or modify it
  ~ under the terms of the GNU Lesser General Public License as
  ~ published by the Free Software Foundation; either version 2.1 of
  ~ the License, or (at your option) any later version.
  ~
  ~ This software is distributed in the hope that it will be useful,
  ~ but WITHOUT ANY WARRANTY; without even the implied warranty of
  ~ MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU
  ~ Lesser General Public License for more details.
  ~
  ~ You should have received a copy of the GNU Lesser General Public
  ~ License along with this software; if not, write to the Free
  ~ Software Foundation, Inc., 51 Franklin St, Fifth Floor, Boston, MA
  ~ 02110-1301 USA, or see the FSF site: http://www.fsf.org.
  -->

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

## platform.xml
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
        <connection-url>jdbc:oracle:thin:@192.168.178.102:1521/orcl</connection-url>
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
copy the driver jar to ```%SERVER_HOME\modules\com\oracle\main```

## module.xml
create the file module.xml in ```%SERVER_HOME\modules\com\oracle\main``` with the following content. Check if resource root path matches the version of the downloaded driver jar.

```
<?xml version="1.0" encoding="UTF-8"?>

<!--
  ~ JBoss, Home of Professional Open Source.
  ~ Copyright 2010, Red Hat, Inc., and individual contributors
  ~ as indicated by the @author tags. See the copyright.txt file in the
  ~ distribution for a full listing of individual contributors.
  ~
  ~ This is free software; you can redistribute it and/or modify it
  ~ under the terms of the GNU Lesser General Public License as
  ~ published by the Free Software Foundation; either version 2.1 of
  ~ the License, or (at your option) any later version.
  ~
  ~ This software is distributed in the hope that it will be useful,
  ~ but WITHOUT ANY WARRANTY; without even the implied warranty of
  ~ MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU
  ~ Lesser General Public License for more details.
  ~
  ~ You should have received a copy of the GNU Lesser General Public
  ~ License along with this software; if not, write to the Free
  ~ Software Foundation, Inc., 51 Franklin St, Fifth Floor, Boston, MA
  ~ 02110-1301 USA, or see the FSF site: http://www.fsf.org.
  -->

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

# Tomcat and Postgres

## server.xml  

Location: ```%SERVER_HOME\conf\server.xml```

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

