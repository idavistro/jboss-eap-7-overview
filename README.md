# JBOSS EAP 7 Overview

## Objetivo
Realizar una descripcion de los principales elementos y configuracion con los que cuenta el Servidor de Aplicaciones JBoss EAP.
JBOSS EAP es un servidor de aplicaciones JAVA EE 7, modular y open source, basado en el proyecto comunitario Wildfly, algunas de sus principales caracteristicas son:
 * Configuracion Simplificada: Todas las configuraciones se encuentra en un archivo XML; para el modo Standalone se utiliza el archivo Standalone.xml y para el modo Domain se utilizan dos archivos domain.xml y host.xml. La unica diferencia entre estas dos configuraciones es la centralizacion en la administración de todas las caracteristicas con las que se configura el servidor de aplicaciones; algunas como seguridad, transaccionalidad, intercambio de mensjaes, clusterizacion, alta disponibilidad, y otras tecnologicas Java EE, están disponibles tanto en standalone como en domain mode.
 * Herramientas de Gestion: Es posible realizar las configuraciones utilizando cualquiera de las siguientes harremientas:
   * XML : Es posible modificar directamente el archivo de configuracion, aunque lo mas recomendado es realizar las modificaciones utilizando las otras herramientas, ya que los cambios pueden sobreescribirse si no se han guardado correctamente.
   * Command Line Interface (CLI) : Como su nombre lo indica, por medio de una interfaz en linea de comando es posible realizar todas las configuraciones escribiendo comandos que posteriormente se ven reflejados en el archivo XML. Además, es posible escribir una serie de comandos y todas sus configuraciones, por ejemplo la creacion de un datasource, en un archivo y enviarlo a la linea de comandos CLI, lo que nos evita errores y permite automatizar todas las operaciones. 
   * Web  Interface : Por medio de una interface WEB se traduce lo leido en el archivo XML y se muestra en una interfaz gráfica que es mas amigable.

## Instalacion
Para realizar la instalación, existen tres maneras:
1. Zip File: El EAP se distribuye como un archivo comprimido que unicamente tiene que ser extraido en la carpeta desde donde se va a ejecutar. El archivo contiene librerias, archivos de configuracion y scripts que permiten iniciar el servidor de aplicaciones.
>$ unzip jboss-eap-7.0.0.zip
* Para realiza la desinstalación se ejecuta el comando:
>'# rm -rf jboss-eap-7.0
1. RPM: También se encuntra disponible como un paquete RPM para los usuarios con una suscripción disponible en el sub-canal JBOSS Enterprise Application Platform, en el grupo JBoss Enterprise Application Platform. Este metodo de instalación solo es relevante para el Sistema Operativo RHEL
>'# yum groupinstall jboss-eap7
* Para realiza la desinstalación se ejecuta el comando:
>'# yum groupremove jboss-eap7
1. Instalador GUI: Es una aplicacion basada en Java que proporciona un entorno de configuracion gráfica que ayuda al usuario paso a paso en las diversas actividades. Asi mismo, puede proporcionar una consola de texto y por lla flexibilidad que tiene, es posible enviar configuraciones especficas en un archivo XML permitiendo que la instalación  en múltiples configuraciones de una manera automatizada.
>$ java -jar jboss-eap-7.0.0-installer.jar
* Para realiza la desinstalación se ejecuta el comando:
>'# yum groupremove jboss-eap7

### Iniciar y Detener el Servidor de Aplicaciones EAP
Tanto scripts en Unix Shell, como scripts Batch Windows se encuentran en la instalación de EAP. Los archivos para cada una de las configuraciones son *standalone.sh* y *domain.sh* que se pueden encontrar en la carpeta *bin* en donde se realizó la instalación.
>$ ${JBOSS_HOME}/bin/standalone.sh </br>
>$ ${JBOSS_HOME}/bin/domain.sh </br>

![StartEAP](/images/JBoss_EAP_02.png)

![TestEAP](/images/JBoss_EAP_06.png)

Para detener el servidor se puede realizar algunas de las siguientes acciones:
* Interrumpiendo el proceso con el comando *CTRL+C*
* Terminando el proceso responsable de la instancia del servidor, con el comando *kill* en sistemas Unix, o desde el Administrador de Tareas en sistemas windows
* Utilizando la herramienta CLI

![StopEAP](/images/JBoss_EAP_04.png)

Las principales carpetas en el directorio de instalación son:
* bin: Contiene los scripts utilizados para iniciar el servidor, ejecutar la herramienta de administracion CLI y otras utilidades para desarrolladores
* domain: Contiene configuraciones y archivos para ejecutar EAP en mod domain
* standalone: Contiene configuraciones y archivos para ejecutar EAP en modo standalone
* modules: Contiene la mayoria del codigo que implementa servicios JEE en EAP

### Subsistemas y Perfiles
El una configuracion standalone solo existe una definicion de perfil sencilla y anónima. En cambio en una configuracion domain mode existen 4 perfiles predefinidos que contemplan la mayoria de los casos para las aplicaciones desplegadas en EAP:

![SubsystemsAndProfles](/images/JBoss_EAP_07.png)

### Creacion de Usuarios
El instalador de EAP crea usuarios administrativos durante el proceso de instalacion, pero si es necesario crear usuarios adicionales o si se utilizo otra forma de instalacion (por ejemplo descomprimiendo el zip) es necesario ejecutar el script *add-user.sh* que se encuentra ubicado en la carpeta *JBOSS_HOME/BIN*.

![SubsystemsAndProfles](/images/JBoss_EAP_05.png)

Existen dos tipos distingos de usuario que pueden configurarse:
* Administrativos: Se utiliza para ingresar a las herramientas administrativas. Las credenciales son almacenadas en el archivo *JBOSS_HOME/standalone/configuration/mgmt-user.properties*, con el password encriptado.
* Aplicacion: Se utiliza por las aplicaciones Java EE y JAAS o similares para administrar los usuarios. Las credenciales son almacenadas en el archivo *JBOSS_HOME/standalone/configuration/app-user.properties*

```sh
#
# Properties declaration of users for the realm 'ManagementRealm' which is the default realm
# for new installations. Further authentication mechanism can be configured
# as part of the <management /> in standalone.xml.
#
# Users can be added to this properties file at any time, updates after the server has started
# will be automatically detected.
#
# By default the properties realm expects the entries to be in the format: -
# username=HEX( MD5( username ':' realm ':' password))
#
# A utility script is provided which can be executed from the bin folder to add the users: -
# - Linux
#  bin/add-user.sh
#
# - Windows
#  bin\add-user.bat
# On start-up the server will also automatically add a user $local - this user is specifically
# for local tools running against this AS installation.
#
# The following illustrates how an admin user could be defined, this
# is for illustration only and does not correspond to a usable password.
#
#admin=2a0923285184943425d1f53ddd58ec7a
jbossadm=9e9950eb85c96ecc517598b3b7bd897f
#
#$REALM_NAME=ManagementRealm$ This line is used by the add-user utility to identify the realm name already used in this file.
#

```

## Configuracion Modo Standalone
En el modo Standalone se representa una sola instance del servidor con un solo archivo de configuracion llamadao *standalone.xml*. En esta modo, únicamente se cuenta con un *profile* definido por default y es en donde se definen los *subsystems* y sus respectivas configuraciones.

Después de la instalacion de EAP, el folder *standalone* contiene las siguientes carpetas:
* configuration/: Contiene los archivos de configuracion, ademas de una subcarpeta llamada *standalone_xml_history* en donde se encuentra un histórico de los archivos de configuracion. Además, se encuentra el archivo de propiedades *logging.properties* para la configuracion de logs, para usuarios el archivo *mgmt-user.properties*, y para la definición el archivo *mgmt-groups.properties*.
* deployments/: Contiene los archivos de las aplicaciones Java EE desplegadas, tales como EAR, WAR y JAR.
* lib/: Se utiliza para desplegar archivos JAR comunes, aunque lo mas recomendado es que las librerias Java (JAR), se instalen como módulos, por lo que esta carpeta comunmente se encuentra vacía.

Después de que se inicia el servidor por primera vez, las siguentes carpetas son creadas:

* data/: Contiene la información utilizada por los subsistemas, tales como mensajes encolados o database in-memory
* log/: Carpeta por default para almacenar los archivos de log, incluyendo *gc.log.0.current* y *server.log*
* tmp/: Archivos temporales, tales como mecanimos shared-key utilizados por CLI, para autenticar un usuario local en el servidor.

### Ejecución EAP desde una Ubicación Custom
La configuracion y los archivos de datos pueden existir en lugares distintos a los de la instalación. Esto permite mantener las configuraciones separadas de la instalación, obteniendo así la posibilidad de realizar upgrades mas fácilmente hacia nuevas versiones de EAP. También brinda la capacidad de ejecutar múltiples instancias del servidor en una misma máquna.

Por ejemplo se puede ejecutar el siguiente comando para inicializar un servidor en otra ruta:

>$ ./standalone.sh -Djboss.server.base.dir=/path/to/base/directory -Djboss.home.dir=/path/to/home/directory

En donde las variables representan:
* jboss.home.dir: El directorio base en donde se realizó la instalación.
* jboss.server.base.dir: El directorio en donde se realizo la copia del contenido de la carpeta standalone

Para cambiar el puerto en donde se levanta el servidor, y poder múltiples instancias ejecutandose se puede ejecutar el siguiente comando. Su función es sumar el valor *port-offset* a los puertos definidos en el archivo standalone.xml:
>$ ./standalone.sh -Djboss.socket.binding.port-offset=10000

![StandaloneOffsetPort](/images/JBoss_EAP_08.png)

### Configuracion del archivo standalone.xml
Como se comento anteriormente, todas las configuraciones realizadas en el servidor son almacenadas en un solo archivo que se encuentra en la carpeta *JBOSS_HOME/standalone/configuration/standalone.xml*. Su estructura general es la siguiente:

```XML
<server xmlns="urn:jboss:domain:4.1">
  <extensions>
    ...list of extensions here
  </extensions>
  <system-properties>
    ...system properties defined here
  </system-properties>
  <management>
    ...management interfaces defined here
  </management>
  <profile>
    ...list of subsystems and their configurations
  </profile>
  <interfaces>
    ...interface definitions
  </interfaces>
  <socket-binding-group>
    ...socket binding definitions
  </socket-binding-group>
  <deployments>
    ...deployed applications go here
  </deployments>
</server>
```

#### Extensions
Son módulos que extienden las funcionalidades core del servidor. Un módulo es un bundle compuesto por librerias desarrolladas en Java y con archivos de configuración XML. Una extensión define un o ms subsistemas basados en ese módulo. Dentro del tag `<extensions>`, los elementos que se van a utilizar se encuentran en el tag `<extension>` . Por ejemplo para agregar la siguiente extensión *ejb3*, se agregan lo siguiente:

```XML
<extensions>
  <!-- list all extensions that you want made available to this server -->
  <extension module="org.jboss.as.clustering.infinispan"/>
  <extension module="org.jboss.as.deployment-scanner"/>
  <extension module="org.jboss.as.ejb3"/>
  <extension module="org.jboss.as.jpa"/>
</extensions>
```
Cada extensión debe estar declarada en el archivo standalone.xml y cada modulo debe estar almacenado en *JBOSS_HOME/modules/system/layers/base*

#### Management Interfaces
El manejo de interfaces permite a clientes conectarse de manera remota al EAP. Se exponen dos tipos de interfaces:
1. HTTP Interface: Proporciona acceso a la consola de administración
1. Native Interface: Permite la ejecucion de operaciones administrativas a través de un protocolo binario propietario. este tipo de interfaz es utilizado por la herramienta CLI.

Los elementos que integran esta sección son los siguientes:

```XML
<management>
  ...
  <management-interfaces>
    <native-interface security-realm="ManagementRealm">
        <socket-binding native="management-native"/>
    </native-interface>
    <http-interface security-realm="ManagementRealm">
      <socket-binding http="management-http"/>
    </http-interface>
  </management-interfaces>
  ...
</management>
```
Los elementos *management-native* y *management-http* de tipo socket-binding se especifican en otras secciones en la sección `<socket-binding-group>`, que es en donde se definen tanto el puerto como el host.

Además es posible establecer los usuarios que pueden accesar remotamente con las siguientes lineas, en donde la sección `<security-realm>` con el tag *ManagementRealm* es utilizado en ambas interfaces y se relacionan con el contenido del archivo *mgmt-user.properties* en donde son almacenadas las credenciales:

```XML
<management>
  ...
  <security-realms>
    <security-realm name="ManagementRealm">
      <authentication>
        <local default-user="$local" skip-group-loading="true"/>
        <properties path="mgmt-users.properties" relative-to="jboss.server.config.dir"/>
      </authentication>
      <authorization map-groups-to-roles="false">
        <properties path="mgmt-groups.properties" relative-to="jboss.server.config.dir"/>
      </authorization>
    </security-realm>
    <security-realm name="ApplicationRealm">
      <authentication>
        <local default-user="$local" allowed-users="*" skip-group-loading="true"/>
        <properties path="application-users.properties" relative-to="jboss.server.config.dir"/>
      </authentication>
      <authorization>
        <properties path="application-roles.properties" relative-to="jboss.server.config.dir"/>
      </authorization>
    </security-realm>
  </security-realms>
  ...
</management>
```
### Profiles & Subsystems
Un perfil es una collección de subsistemas. Un subsistema es en donde se configuran las extensiones de la configuracion standalone del EAP. 

El elemento `<profile>` contiene una colección de elementos hijos `<subsystem>` y dentro de estos elementos existe una única configuración para una extensión en particular. Algunas definiciones de subsistemas no necesitan configuraciones adicionales, por ejemplo *jsf* y *jaxrs*.

```XML
<profile>
  <subsystem xmlns="urn:jboss:domain:datasources:4.0">
    <datasources>
      <datasource jndi-name="java:jboss/datasources/ExampleDS" pool-name="ExampleDS" enabled="true" use-java-context="true">
        <connection-url>jdbc:h2:mem:test;DB_CLOSE_DELAY=-1;DB_CLOSE_ON_EXIT=FALSE</connection-url>
        <driver>h2</driver>
        <security>
          <user-name>sa</user-name>
          <password>sa</password>
        </security>
      </datasource>
      <drivers>
        <driver name="h2" module="com.h2database.h2">
          <xa-datasource-class>org.h2.jdbcx.JdbcDataSource</xa-datasource-class>
        </driver>
      </drivers>
    </datasources>
  </subsystem>
</profile>
        
```
### Interfaces
Una interface es el nombre lógico para una interfaz de red, direccion IP, o el nombre de un host relacionados a un socket. 

```XML
<interfaces>
  <interface name="management">
    <inet-address value="${jboss.bind.address.management:127.0.0.1}"/>
  </interface>
  <interface name="public">
     <inet-address value="${jboss.bind.address:127.0.0.1}"/>
  </interface>
</interfaces>
        
```
Para enviar el valor al momento de iniciar el servidor, se utiliza la propiedad -bmanagement

>$ ./standalone.sh -bmanagement 127.0.0.1

### Socket Binding Group
Es una colección de socket bindings, los cuales permiten definir los puertos necesarios a utilizar por la instancia del EAP. 

```XML
<socket-binding-group name="standard-sockets" default-interface="public" port-offset="${jboss.socket.binding.port-offset:0}">
  <socket-binding name="management-http" interface="management" port="${jboss.management.http.port:9990}"/>
  <socket-binding name="management-https" interface="management" port="${jboss.management.https.port:9993}"/>
  <socket-binding name="ajp" port="${jboss.ajp.port:8009}"/>
  <socket-binding name="http" port="${jboss.http.port:8080}"/>
  <socket-binding name="https" port="${jboss.https.port:8443}"/>
  <socket-binding name="txn-recovery-environment" port="4712"/>
  <socket-binding name="txn-status-manager" port="4713"/>
  <outbound-socket-binding name="mail-smtp">
    <remote-destination host="localhost" port="25"/>
  </outbound-socket-binding>
</socket-binding-group>
        
```
### Deployments
En esta sección se enlistan las aplicaciones desplegadas en el sevidor EAP standalone.

```XML
<deployments>
  <deployment name="helloworld.war" runtime-name="bookstore.war">
    <content sha1="e1e57cb8b89371794d6c7e80baeb8bf0e3da4fcf"/>
  </deployment>
  <deployment name="example.war" runtime-name="example.war" enabled="false">
    <content sha1="0a07b224819ce516b231b1afba0eadc45b272298"/>
  </deployment>
  <deployment name="version.war" runtime-name="version">
    <fs-exploded path="deploying-filesystem/version.war" relative-to="labs"/>
  </deployment>
</deployments>
```

En el ejemplo descrito, existen 3 aplicaciones desplegadas: *helloworld.war, example.war* y *version.war*, en donde las primeras dos aplicaciones son administradas por el EAP y la tercera fue desplegada con un metodo no conocido.

El elemento `<content>` represena el valor de un algoritmo de hash seguro (SHA), el cual es utilizado internamente como un identificador único del despliegue.

Los despliegues administrados se almacenan en la carpeta *JBoss_HOME/data/content*, en donde se crea un subfolder que comiensa con los dos primeros caracteres del codigo SHA1

```TXT
[jgamboag@localhost labs]$ tree -d $JBOSS_HOME/data/content/
standalone-instance/data/content/
├── 0a
│   └── 07b224819ce516b231b1afba0eadc45b272298
└── e1
    └── e57cb8b89371794d6c7e80baeb8bf0e3da4fcf
```

Existen 3 maneras de desplegar aplicaciones:
1. Desde la Consola de Administracion WEB: clic en la sección superior *Deployments* del menú.

![StandaloneOffsetPort](/images/JBoss_EAP_09.png)

  * Seleccionar el archivo a desplegar.
  
![StandaloneOffsetPort](/images/JBoss_EAP_10.png)

  * Definir las opciones de *Name* (identificador del despliegue único por aplidación), *Runtime Name* (define el contexto de la aplicación) y *Enabled* (permite que el despliegue inicie automáticamente cuando inicia la instancia)

![StandaloneOffsetPort](/images/JBoss_EAP_11.png)

2.  Despliegue utilizando CLI: Es necesario ejecutar el comando *deploy* para realizar el despliegue de la aplicación. El comando tiene los siguientes argumentos:

  * file_path
  * --url
  * --name
  * --runtime-name
  * --force
  * --disabled

> [standalone@localhost:9990 /] deploy /home/jgamboag/Documentos/RedHat/EAP_Practice/version.war --name=version.war

![StandaloneOffsetPort](/images/JBoss_EAP_13.png)

3. Despligue mediante el File System Deployer: Es un subsistema del EAP que escanea cierto directorio por la existencia de aplicaciones Java EE. Para realizar el despliegue manual es necesario colocar la aplicacion en la carpeta *JBOSS_HOME/deployments*, por medio de marcadores es posible realizar las siguientes acciones:
  
  * .dodeploy: Es creado por el usuario y lanza una orden para comenzar con el despliegue
  * .deployed: Indica que la aplicacion ha sido desplegada
  * .isdeploying: Indica que la aplicación esta siendo desplegada
  * .failed: Indica que falló la publicación
  * .isundeploying: Indica que la aplicación se esta eliminando
  * .undeployed: Indica que la aplicacion se elimino.
  * .skipdeployed: Es creado por el usuario e indica que la aplicacion no debe ser desplegada.
  * .pending: Indica que la aplicacion tiene que desplegarse pero el servidor no ha recibido la instruccion de hacerlo.
  
## Configuracion Modo Domain Mode
Esta esta configuracion es posible manejar multiples instancias del servidor, publicaciones en multiples hosts desde un solo lugar centralizado. Para lograr la centralizacion se cuenta con un proceso **domain controller (tambien llamado master** que actua actua como un punto de control y se comunica con varios **host controllers (tambien llamados slaves)** en el dominio gestionado. Todos los hosts comparten politicas de administracion y el servidor controlador se asegura que todos estén configurados así.
Los archivos en donde se hace la configuración y que solo existe en el domain controller, se llama *domain.xml*, y en cada uno de los host controlers se configura el archivo *host.xml*

Algunas definiciones imporantes a considerar son:
  * Domain: Una conjunto de instancias  del EAP
  * Domain Controller: Es un solo proceso que actua como manejador central de la administracion. Tambien llamado *master*
   * Server: Una instancia del EAP ejecutandose sobre un proceso JVM.
   * Host controler: Un proceso que se encuentra en un host y replica la configuracion de información, status en tiempo de ejecución y la administracion de comandos en una computadora en especfico. Tambien llamado *slave*
  * Process controller: Un proceso ejecutandose en una máquina que inicia el host controller y las instancias del servidor.
  * Host: Un conjunto de procesos iniciados por el mismo process controller; un host controller puede tener una o 0 instancias del servidor
  * Server group: Una conjunto de servidores que son administrados y manejados como uno solo.
  * Profile: El nombre que se establece para las configuraciones de los subsistemas en el EAP

### Configuracion del Domain Controler
La configuración se divide en dos archivs
* host.xml: Archivo de configuración del host controller.
* domain.xml: Archivo de configuración para el domain controller, en donde se definen los perfiles disopnibles. 

El archivo host.xml, se muestra como sigue:

```XML
<?xml version="1.0" ?>
<host xmlns="urn:jboss:domain:4.1" name="mydomainmaster">
  ...
  </management>
  <domain-controller>
    <local/>
  </domain-controller>
  <interfaces>
  ...
</host>
```

El elemento `<domain-controller> ` infroma al host controller en donde puede encontrar al domain controller. En caso de que host controller sea el *master* se utiliza el elemento `<local/> ` . 

Para configurar un host controller como *slave*, es necesario especificarlo en el archivo host.xml

```XML
<?xml version="1.0" ?>
<host xmlns="urn:jboss:domain:4.1" name="mydomainmaster">
  ...
  <domain-controller>
    <remote security-realm="ManagementRealm">
      <discovery-options>
        <stati c-discovery name="primary" protocol="${jboss.domain.master.protocol:remote}" host="${jboss.domain.master.address}" port="${jboss.domain.master.port:9999}"/>
        </discovery-options>
    </remote>
  </domain-controller>
  ...
```

El elemento `<remote> ` informa al host controller que es un slave y que los elementos hijos especifican en donde encontrar al master.

Para iniciar el servidor unicamente es necesario ejecutar el comando, en el directorio */bin* de la carpeta de la instalación.:

> $ ./domain.sh

Para realizar una copia y poder realizar modificaciones en otra carpeta se ejecuta el comando:

> ./domain.sh -Djboss.domain.base.dir=/home/jgamboag/Documentos/RedHat/EAP_Practice/DomainMode/domain --domain-config=mydomain.xml --host-config=myhost.xml

A diferencia del modo standalone, en la consola de administración se pueden observar en el menu superior el tab *Runtime*, con operaciones adicionales tales como configuracion de hosts, server groups y server instances.

![StandaloneOffsetPort](/images/JBoss_EAP_14.png)

### Configuracion del Host Controller
La estructura general del archivo host.xml se define por el XML Schema *JBOSS_HOME/docs/schema/wildfly-config_4_1.xsd*

```XML
<host name="my_hostname" xmlns="urn:jboss:domain:4.1">
  <extensions>
    ... host controller extensions
  </extensions>
  <system-properties>
    ... for defining system properties
  </system-properties>
  <paths>
    ... for defining filesystem paths
  </paths>
  <vault>
    ... for storing encrypted passwords
  </vault>
  <management>
    ... the management interfaces and their security settings appear here
  </management>
  <domain-controller>
    ... the settings for how to connect to the Domain Controller
  </domain-controller>
  <interfaces>
    ... interfaces are defined here
  </interfaces>
  <jvms>
    ... JVMs are defined here
  </jvms>
  <servers>
    ... servers are defined here
  </servers>
  <profile>
    ... subsystems configurations for host controller extensions
  </profile>
</host>
```
Los únicos elementos mandatorios son:  `<management>`, `<domain-controller>` y `<host>`, y la descripción de los elementos es la siguiente:
* `<extension>` : Declara las extensiones que necesitan ser cargadas por el host controler
* `<system-properties>` : Define las propiedades del sistema especificas del host 
* `<paths>` : Define el path del filesystem utilizado para la configuracion de los subsistemas.
* `<vault>` : Define un security vault, es decir el lugar en donde son almacenados los passwords encriptados
* `<management>` : Define las interfaces de administracion disponibles, así como las configuraciones de seguridad y auditoria  
* `<interfaces>` : Define las direcciones IP de los servicis y los subsistemas
* `<domain-controller>` : Especifica en donde se encuentra el host controller y el domain controller, o si el mismo es el domain-controller
* `<jvms>` : Define multiples configuraciones de una Java Virtual Machine que pueden ser referenciadas por diferentes grupos de servidores y ser identificados individualmente por instancias del servidor
* `<servers>` : Define las instancias del servidor en el host controller. 
* `<profile>` : Especifica la configuracion de los subsistemas en los elementos `<extensions>`

A continuación se muestra un ejemplo de un archivo host.xml

```XML
<host name="myhost" xmlns="urn:jboss:domain:4.1">
  ...
  <jvms> 
    <jvm name="default">
      <heap size="64m" max-size="128m"/>
    </jvm>  
    <jvm name="myhost-jvm">
      <heap size="1024m" max-size="2048m"/>
    </jvm>
  </jvms>
  <servers>
    <server name="server1" group="group1">
      <jvm name="default"/>
    </server>
    <server name="server2" group="group2">
      <jvm name="myhost-jvm"/>
      <socket-bindings port-offset="200"/>
    </server>
  </servers>
</host>
```

## Datasources
Los pasos necesarios para poder realizar la configuracion de un Subsistema Datasoruce, son los siguientes:
1. Instalar el driver JDBC que se necesita
1. Definir un datasource dentro del subsistema *datasource* en el archivo de configuracion.

**Database Connection Pool**: Cuando se manejan data-sources es necesario poder controlar el trafico y la concurrencia en las conexiones hacia la base de datos, ya que puede ocasionar un potencial cuello de botella provocando problemas de performance. Para mitigar esta problemática , EAP genera un database connection pool para cada una de las bases de datos conectadas con las aplicaciones, por lo que cuando una aplicacion necesita de una conexión, el EAP la proporciona evitando así la sobretrabajo de abrir y cerrar conexiones para cada petición. El tamaño del pool de conexiones puede ser configurado para cada uno de los data source.

### Configuracion de un Driver JDBC
Un *JDBC Driver* es un componente que las aplicacines Java utilizan para comunicarse con las bases de datos. Un JDBC driver es empaquetado dentro de un archivo JAR y contiene las clases de la definición del driver. Los JDBC driver están disponibles a través de los fabricantes, por ejemplo MySQL o PostgreSQL, por lo que para utilizarlos lo primero es instalarlo.

Para realizar la instalación con CLI se ejecuta el siguiente comando:

>[disconnected /] module add --name=<module_name> --resources=<JDBC_Driver> --dependencies=<library1>,< library2>,...

Por ejemplo para un driver MySQL se ejecuta el siguiente comando:

>[disconnected /] module add --name=com.mysql \
--resources=/opt/jboss-eap-7.0/bin/mysql-connector-java.jar \
--dependencies=javax.api,javax.transaction.api

Por lo que la definición del modulo es:

```XML
<?xml version="1.0" ?>
<module xmlns="urn:jboss:module:1.1" name="com.mysql">
  <resources>
    <resource-root path="mysql-connector-java.jar"/>
  </resources>
  <dependencies>
    <module name="javax.api"/>
    <module name="javax.transaction.api"/>
  </dependencies>
 </module>
 ```
 
Despues de instalar el driver como módulo, es necesario crear la definicion del `<driver>` en la sección del subsistema data source del archivo de configuración.
Con CLI, se puede realizar ejecutando el siguiente comando:

>/subsystem=datasources/jdbc-driver=<driver_name>:add\
(driver-module-name=<module_name>,driver-name=<unique_driver_name>)

Para un managed domain, la sintaxis es la siguiente:

>/profile=<profile_name>/subsystem=datasources/jdbc-driver=<driver_name>:add\(driver-module-name=<module_name>,driver-name=<unique_driver_name>)

El comando específico para el driver MySQL, sería el siguiente:

>[domain@172.25.250.254 /] /profile=default/subsystem=datasources/jdbc-driver=mysql:add(\driver-module-name=com.mysql,\driver-name=mysql\)

#### Configuracion de un DataSource
Los datasource se configuran en los archivos de configuracion (*domain.xml* y 
