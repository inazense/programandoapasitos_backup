---
title: Procesos y servicios. Programación segura V. Comunicación segura con Java
description: 
author: Inazio Claver
date: 2016-03-26 12:10:00 +0800
categories: [Java]
tags: [procesos y servicios, java, teoria]
pin: false
math: false
mermaid: false
#image:
#  path: /commons/devices-mockup.png
#  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
#  alt: Responsive rendering of Chirpy theme on multiple devices.
---

## JSSE

**JSSE** (Java Secure Socket Extension) es un conjunto de paquetes que permiten el desarrollo de aplicaciones seguras en Internet. Proporciona un marco y una implementación para la versión **Java** de los protocolos **SSL** y **TSL** e incluye funcionalidad de encriptación de datos, autenticación de servidores, integridad de mensajes y autenticación de clientes.
Con **JSSE**, los desarrolladores pueden ofrecer intercambio seguro de datos entre un cliente y un servidor que ejecuta un protocolo de aplicación, tales como **HTTP**, **Telnet** o **FTP**, a través de **TCP/IP**.
Las clases de **JSSE** se encuentran en los paquetes ```javax.net``` y ```javax.net.ssl```.

### SSL

Las clas **SSLSocket** y **SSLServerSocket** representan sockets seguros y son derivadas de las ya familiares **Socket**  y **ServerSocket** respectivamente.

JSSE tiene dos clases **SSLServerSocketFactory** y **SSLSocketFactory** para la creación de sockets seguros. No tienen constructor, se obtienen a través del método estatico ```getDefault()```.

Para obtener un socket servidor seguro o **SSLServerSocket**

```java
SSLServerSocketFactory sfact = (SSLServerSocketFactory)
SSLServerSocketFactory.getDefault();
SSLServerSocket servidorSSL = (SSLServerSocket)
sfact.createServerSocket(puerto);
```

El método ```createServerSocket(int puerto)``` devuelve un **socket** de servidor enlazado al puerto especificado. Para crear un **SSLSocket**:

```java
SSLSocketFactory sfact = (SSLSocketFactory)
SSLSocketFactory.getDefault();
SSLSocket Cliente = (SSLSocket) sfact.createSocket(Host, puerto);
```

#### Archivo ServidorSSL.java

```java
import java.io.*;
import javax.net.ssl.*;

public class ServidorSSL {
     
      public static void main(String[] arg) throws IOException {
           
           
            //System.setProperty("javax.net.ssl.keyStore", System.getProperty("user.dir") + "\\AlmacenSSL");
            //System.setProperty("javax.net.ssl.keyStorePassword", "1234567");
           
           
            int puerto = 6000;
            SSLServerSocketFactory sfact = (SSLServerSocketFactory) SSLServerSocketFactory.getDefault();
            SSLServerSocket servidorSSL = (SSLServerSocket) sfact.createServerSocket(puerto);
            SSLSocket clienteConectado = null;
            DataInputStream flujoEntrada = null; //FLUJO DE ENTRADA DE CLIENTE
            DataOutputStream flujoSalida = null; //FLUJO DE SALIDA AL CLIENTE
           
            for (int i = 1; i < 5; i++) {
                 
                  System.out.println("Esperando al cliente " + i);
                  clienteConectado = (SSLSocket) servidorSSL.accept();
                  flujoEntrada = new DataInputStream(clienteConectado.getInputStream());
                  // EL CLIENTE ME ENVIA UN MENSAJE
                  System.out.println("Recibiendo del CLIENTE: " + i + " \n\t" + flujoEntrada.readUTF());
                  flujoSalida = new DataOutputStream(clienteConectado.getOutputStream());
                  // ENVIO UN SALUDO AL CLIENTE
                  flujoSalida.writeUTF("SaIudos al cliente del servidor");
                 
            }
           
           
            // CERRAR STREAMS Y SOCKETS
            flujoEntrada.close();
            flujoSalida.close();
            clienteConectado.close();
            servidorSSL.close();
           
      }
     
}
```

Crea una conexión sobre un socket servidor seguro y que atenderá hasta cuatro conexiones de clientes que se identificarán con un certificado válido. El servidor espera las conexiones, de cada cliente que se conecta recibe un mensaje y a continuación le envía un saludo.

#### Archivo ClienteSSL.JAVA

```java
import java.io.*;
import javax.net.ssl.*;

public class ClienteSSL {
     
      public static void main(String[] args) throws Exception {
           
           
            //System.setProperty("javax.net.ssl.trustStore", System.getProperty("user.dir") + "\\UsuarioAlmacenSSL");
            //System.setProperty("javax.net.ssl.trustStorePassword", "890123");
           
            String Host = "localhost";
            int puerto = 6000;
           
            System.out.println("PROGRAMA CLIENTE INICIADO....");
            SSLSocketFactory sfact = (SSLSocketFactory) SSLSocketFactory.getDefault();
            SSLSocket Cliente = (SSLSocket) sfact.createSocket(Host, puerto);
           
            // CREO FLUJO DE SALIDA AL SERVIDOR
            DataOutputStream flujoSalida = new DataOutputStream(Cliente.getOutputStream());
           
            // ENVIO UN SALUDO AL SERVIDOR   
            flujoSalida.writeUTF("Saludos al SERVIDOR DESDE EL CLIENTE");
           
            // CREO FLUJO DE ENTRADA AL SERVIDOR
            DataInputStream flujoEntrada = new DataInputStream(Cliente.getInputStream());
           
            // EL SERVIDOR ME ENVIA UN MENSAJE
            System.out.println("Recibiendo del SERVIDOR: \n\t" + flujoEntrada.readUTF());
           
            /*------------------------------------------------------------------------------
            //Información sobre la sesión SSL
            SSLSession session = ((SSLSocket) Cliente).getSession(); 
            System.out.println("Host: "+session.getPeerHost());
            System.out.println("Cifrado: " + session.getCipherSuite());
            System.out.println("Protocolo: " + session.getProtocol());
            System.out.println("IDentificador:" + new BigInteger(session.getId()));
            System.out.println("Creación de la sesión: " + session.getCreationTime());

            X509Certificate certificate = (X509Certificate)session.getPeerCertificates()[0];
            System.out.println("Propietario: " + certificate.getSubjectDN());
            System.out.println("Algoritmo: " + certificate.getSigAlgName());
            System.out.println("Tipo: " + certificate.getType());
            System.out.println("Emisor: " + certificate.getIssuerDN());
            System.out.println("Número Serie: " + certificate.getSerialNumber());
            ------------------------------------------------------------------------------*/
           
            // CERRAR STREAMS Y SOCKETS
            flujoEntrada.close();
            flujoSalida.close();
            Cliente.close();
           
      }
     
}
```

Envía un mensaje al servidor y visualiza el que el servidor le devuelve.

El servidor necesita disponer de un certificado que mostrar a los clientes que se conecten a él. Usaremos la herramienta **keytool** para crearlo, en el ejemplo le damos el nombre de AlmacenSSL y el valor de la clave es 1234567.

```bash
C:>keytool  -genkey -alias claveSSL -keyalg RSA –keystore AlmacenSSL -storepass 1234567
```

Para ejecutar el programa servidor es necesario indicar el certificado que se utilizará

```bash
C:>java -Djavax.net.ssl.keyStore=AlmacenSSL -Djavax.net.ssl.keyStorePassword=1234567 ServidorSSL
```

Antes de ejecutar el programa cliente necesitamos colocar el certificado en el keystore del usuario, para ello lo exportamos a un fichero, le llamamos por ejemplo ```CertificadoSSL.cer```

```bash
C:>keytool -export -alias claveSSL -keystore AlmacenSSL -storepass 1234567 -file Certificado.cer
```

Una vez que tenemos el fichero exportado es necesario incorporarle al nuevo almacenamiento para permitir realizar la validación. A continuación se crea un keystore de nombre ```UsuarioAlmacenSSL``` con la clave 890123 y se incorpora el fichero de certificado ```Certificado.cer```. Esto lo hacemos donde ejecutemos el cliente.

```bash
C:>keytool -import -alias claveSSL -file Certificado.cer –keystore UsuarioAlmacenSSL -storepass 890123
```

Para ejecutar el programa cliente escribimos lo siguientes

```bash
C:>java -Djavax.net.ssl.trustStore=UsuarioAlmacenSSL -Djavax.net.ssl.trustStorePassword=890123 ClienteSSL
```

En estos ejemplos para ejecutar el programa cliente y el servidor hemos establecido las propiedades **JSSE** desde la línea de comandos usando la sintaxis ```–Dpropiedad=Valor```. También se pueden establecer desde el programa usando el método ```System.setProperty(String propiedad, String valor)```.

|Propiedad|Significado|
|---|---|
|javax.net.ssl.keyStore|Ubicación del fichero de almacén de claves de JAva. En Windows, la ruta de acceso especificada debe utilizar barras inclinadas, /, en lugar de barras invertidas, \|
|javax.net.ssl.keyStorePassword|Contraseña para acceder a la calve privada en el fichero de almacén de claves especificado por ```javax.net.ssl.keyStore```|
|javax.net.ssl.trustStore|Ubicación del fichero de almacén de claves que contiene la colección de certificados de confianza. En Windows, la ruta de acceso especificada debe utilizar barras inclinadas, /, en lugar de barras invertidas, \|
|javax.net.ssl.keyStorePassword|Contraseña para abrir el fichero de almacén de claves especificado por ```javax.net.ssl.trustStore```|

En el programa servidor incluimos las siguientes líneas después de definir la variable puerto:

```java
System.setProperty("javax.net.ssl.keyStore", "AlmacenSSL");
System.setProperty("javax.net.ssl.keyStorePassword", "1234567");
```

Y en el programa cliente:

```java
System.setProperty("javax.net.ssl.trustStore", "UsuarioAlmacenSSL");
System.setProperty("javax.net.ssl.trustStorePassword", "890123");
```

Opcionalmente podemos registrar un proveedor SSL en los programas añadiendo esta línea:

```java
java.security.Security.addProvider(new com.sun.net.ssl.internal.ssl.Provider());
```

El método ```getSession()``` de la clase **SSLSocket** devuelve un objeto **SSLSession** con la sesión **SSL** utilizada por la conexión, a partir de ella podemos obtener información como el identificador de la sesión, el cifrado **SSL**, el protocolo, el certificado, etc.

```java
SSLSession session = ((SSLSocket) Cliente).getSession();
System.out.println("Host: "+session.getPeerHost());
System.out.println("Cifrado: " + session.getCipherSuite());
System.out.println("Protocolo: " + session.getProtocol());
System.out.println("IDentificador:" + new BigInteger(session.getId()));
System.out.println("Creación de la sesión: " + session.getCreationTime());
X509Certificate certificate = (X509Certificate)session.getPeerCertificates()[0];
System.out.println("Propietario: " + certificate.getSubjectDN());
System.out.println("Algoritmo: " + certificate.getSigAlgName());
System.out.println("Tipo: " + certificate.getType());
System.out.println("Emisor: " + certificate.getIssuerDN());
System.out.println("Número Serie: " + certificate.getSerialNumber());
```

En el programa servidor para obtener la información del certificado tendríamos que utilizar el método ```getLocalCertificates()``` en lugar de ```getPeerCertificates()```.

## JAAS

**JAAS** (Java Authentication and Authorization Service, Servicio de Autorización y Autenticación de Java) es una interfaz que permite a las aplicaciones Java acceder a servicios de control de autenticación y acceso.

Se puede utilizar para dos propósitos:

1. Para la autenticación de usuarios: para determinar de forma fiable y segura quién está ejecutando nuestro código **Java**
2. Para la autorización de los usuarios: Para asegurarse de que quien lo ejecuta tiene los permisos necesarios para realizar las acciones.

En estos procesos están involucradas las clases e interfaces:

- **LoginContext**, contexto de inicio de sesión: clase para iniciar y gestionar el proceso de autenticación mediante la creación de un Subject. La autenticación se hace llamando al método login().
- **LoginModulo**, módulo de conexión: interfaz para definir los mecanismos de autenticación. Se deben implementar los siguientes métodos: ```iniatilize()```, ```login()```, ```commit()```, ```abort()``` y ```logout()```. Se encarga de validar los datos en un proceso de autenticación.
- **Subject**, clase para representar a un ente autenticable (entidad, usuario, sistema)
- **Principal**, clase que representa los atributos que posee cada **Subject** recuperado una vez que se efectúa el ingreso a la aplicación. Un **Subject** puede contener varios principales.
- **CallBackHandler**, encargado de la interacción con el usuario para obtener los datos necesarios para la autenticación. Se debe implementar el método ```handle()```.

Los paquetes en los que están disponibles las clases e interfaces principales de JAAS son:

- **javax.security.auth.*** que contiene las clases de base e interfaces para los mecanismos de autenticación y autorización.
- **javax.security.auth.callback.*** que contiene las clases e interfaces para definir las credenciales de autenticación de la aplicación.
- **javax.security.auth.login.*** que contiene las clases para entrar y salir de un dominio de aplicación.
- **javax.security.auth.spi.*** que contiene las interfaces para un proveedor de **JAAS** para implementar módulos **JAAS**.

## Autenticación

El proceso básico de autenticación consta de los siguientes pasos

- Creación de una instancia de **LoginContext**, uno o más **LoginModulo** son cargados basándose en el archivo de configuración de **JAAS**.
- La instanciación de cada **LoginModule** es opcionalmente provista con un **CallbackHandler** que gestionará el proceso de comunicación con el usuario para obtener los datos con los que este tratará de autenticarse.
- Invocación del método ```login()``` del **LoginContext** el cual invocará el método ```login()``` del **LoginModule**.
- Los datos del usuario se obtienen por medio del **CallbackHandler**.
- El **LoginModule** comprueba los datos introducidos por el usuario y los valida. Si la validación tiene éxito el usuario queda autenticado.

### Ejemplo

Esquema básico de las clases y ficheros que se van a utilizar en el ejemplo para probar la autenticación básica con JAAS

![Clases para el ejemplo de autenticación](/img/posts/20160326_1.png)

Los ficheros son los siguientes:

- Fichero **EjemploJaasAutenticacion.java**, es la aplicación que será autenticada y autorizada mediante JAAS.
- Fichero **EjemploLoginModule.java**, implementa el módulo **LoginModule** que autentifica a los usuarios mediante su nombre y contraseña.
- Fichero **MyCallbackHandler.java** que implementa **CallbackHandler**. Transfiere la información requerida al módulo **LoginModule**.
- Fichero de autenticación de **JAAS**, ```jaas.config```, donde se configura el módulo de login. Se vincula el nombre EjemploLogin al módulo **LoginModule** de nombre - **EjemploLoginModule**, la palabra required indica que el módulo de conexión asociado debe ser capaz de autenticar al sujeto en todo el proceso de autenticación para poder tener éxito.

```java
EjemploLogin{
     EjemploLoginModule required;
}
```

- Fichero de autenticación **JAAS**, ```policy.config```. Se conceden permisos de lectura sobre las propiedades usuario y clave que se introducirán desde la línea de comandos y el permiso para crear un contexto de inicio de sesión, createLoginContext, al nombre ```EjemploLogin``` vinculado con la clase ```EjemploLoginModule```. El contenido del fichero es el siguiente:

```java
grant {
     permission java.util.PropertyPermission "usuario", "read";
     permission java.util.PropertyPermission "clave", "read";
     permission javax.security.auth.AuthPermission  "createLoginContext.EjemploLogin";
};
```

#### Archivo MyCallbackHandler.java




**¡Salud y coding!**