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



## Configuracion Modo Domain Mode
Esta esta configuracion es posible manejar multiples instancias del servidor, publicaciones en multiples hosts desde un solo lugar centralizado. Para lograr la centralizacion se cuenta con un proceso **domain controller (tambien llamado master** que actua actua como un punto de control y se comunica con varios **host controllers (tambien llamados slaves)** en el dominio gestionado. Todos los hosts comparten politicas de administracion y el servidor controlador se asegura que todos estén configurados así.
Los archivos en donde se hace la configuración y que solo existe en el domain controller, se llama *domain.xml*, y en cada uno de los host controlers se configura el archivo *host.xml*

## Datasources
This demo was created to review some general features of JBoss Fuse 6.1.1 <br/>

# Web Services Detail

We will create four web services which are: <br/><br/>
 * Sum Web Service:  It will be created using **Code first** approach. SOAP request will receive two numbers and return a sum operation. 
The wsdl definition will be available at `http://localhost:{CustomPort}/sum/?wsdl`.<br/>
This is a SOAP Request and SOAP Response example:<br/>
SOAP Request: <br/>
```XML
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:wss="http://wssuma.ws.demos.fuse.redhat.com/">
   <soapenv:Header/>
   <soapenv:Body>
      <wss:sum>
         <arg0>
            <oper1>5</oper1>
            <oper2>1</oper2>
         </arg0>
      </wss:sum>
   </soapenv:Body>
</soapenv:Envelope>
```
SOAP Response: <br/>
```XML
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"  xmlns:wss="http://wssuma.ws.demos.fuse.redhat.com/">
   <soapenv:Header/>
   <soapenv:Body>
      <wss:sumResponse>
         <return>
            <result>6</result>
         </return>
      </wss:sumResponse>
   </soapenv:Body>
</soapenv:Envelope>
```

 * Add Web Service:  It will be created using **Code first** approach. SOAP request will receive two numbers and return an add operation.  The wsdl definition will be available at `http://localhost:{CustomPort}/add/?wsdl`.<br/>
