# JBOSS EAP 7 Overview

## Objetivo
Realizar una descripcion de los principales elementos y configuracion con los que cuenta el Servidor de Aplicsciones JBoss EAP.

## Instalacion
This demo was created to review some general features of JBoss Fuse 6.1.1 <br/>
It was created to help new fuse developers to understand how to create web services using different approaches. This services will execute basic mathematical operations (sum, add, multiply). We will create an additional web service that will standardize request to the other services and act as a proxy too.<br/><br/>

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
