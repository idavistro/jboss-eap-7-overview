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

```TXT
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

Existen dos tipos distingos de usuario que pueden configurarse:
* Administrativos: Se utiliza para ingresar a las herramientas administrativas. Las credenciales son almacenadas en el archivo *JBOSS_HOME/standalone/configuration/mgmt-user.properties*, con el password encriptado.
* Aplicacion: Se utiliza por las aplicaciones Java EE y JAAS o similares para administrar los usuarios. Las credenciales son almacenadas en el archivo *JBOSS_HOME/standalone/configuration/app-user.properties*

## Configuracion Modo Standalone
En el modo Standalone se representa una sola instance del servidor con un solo archivo de configuracion llamadao *standalone.xml*.

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
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:wss="http://wssuma.ws.demos.fuse.redhat.com/">
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
