# JBOSS EAP 7 Overview

## Objetivo
Realizar una descripcion de los principales elementos y configuracion con los que cuenta el Servidor de Aplicsciones JBoss EAP.
JBOSS EAP es un servidor de aplicaciones modular JAVA EE open source, basado en el proyecto comunitario Wildfly, algunas de sus principales caracteristicas son:
 * Configuracion Simplificada: Todas las configuraciones se encuentra en un archivo XML; para el modo Standalone se utiliza el archivo Standalone.xml y para el modo Domain se utilizan dos archivos domain.xml y host.xml.
 * Herramientas de Gestion: Es posible realizar las configuraciones utilizando cualquiera de las siguientes harremientas:
   * XML : Es posible modificar directamente el archivo de configuracion, aunque lo mas recomendado es realizar las modificaciones utilizando las otras herramientas, ya que los cambios pueden sobreescribirse si no se han guardado correctamente.
   * Command Line Interface (CLI) : Como su nombre lo indica, por medio de una interfaz en linea de comando es posible realizar todas las configuraciones escribiendo comandos que posteriormente se ven reflejados en el archivo XML. Además, es posible escribir una serie de comandos y todas sus configuraciones, por ejemplo la creacion de un datasource, en un archivo y enviarlo a la linea de comandos CLI, lo que nos evita errores y permite automatizar todas las operaciones.
Para iniciar la interfaz, es necesario ejecutar el comando `<addr>$EAP_HOME/bin/jboss-cli.sh`. 
   * Web Interface : Por medio de una interface WEB se traduce lo leido en el archivo XML y se muestra en una interfaz gráfica que es mas amigable.

## Instalacion
Para realizar la instalación, existen dos maneras:
1. Ejecutar el configurador
1. Descomprimir el archivo.zip : 

## Configuracion Modo Standalone
This demo was created to review some general features of JBoss Fuse 6.1.1 <br/>

## Configuracion Modo Domain Mode
This demo was created to review some general features of JBoss Fuse 6.1.1 <br/>

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
