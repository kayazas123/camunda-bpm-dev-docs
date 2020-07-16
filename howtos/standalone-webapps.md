# How to Install Standalone Webapps

Make sure to use a vanilla distribution of the application server and follow the [standalone guide](https://docs.camunda.org/manual/latest/installation/standalone-webapplication/) in our docs.

**Heads-Up**  
The job executor is disabled by default. In order to perform batch operations you have to enable the job executor in the engine configuration.

## Download a Vaniall Application Server

### Tomcat
| Version | Project Website | Nexus |
| ------- |:-------------:| -----:|
| 9       | [Tomcat 9 Software Downloads](https://tomcat.apache.org/download-90.cgi) |   X   |


### JBoss
| Version | Project Website | Nexus |
| ------- |:-------------:| -----:|
| 6-7       | [JBoss Downloads](https://jbossas.jboss.org/downloads) |   [jboss-eap-dist](https://app.camunda.com/nexus/service/rest/repository/browse/thirdparty-internal/org/jboss/eap/jboss-eap-dist/)   |

### Wildfly
| Version | Project Website | Nexus |
| ------- |:-------------:| -----:|
| 8-18       | [Wildfly Downloads](https://wildfly.org/downloads/) |   X   |

### Oracle Weblogic
Use the portainer templates

### IBM Webspherre
Use the portainer templates