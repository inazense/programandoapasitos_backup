---
title: Procesos y servicios. Programación segura III. Seguridad en entornos Java
description: 
date: 2016-03-24 12:10:00 +0800
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

Antes de que la **JVM** comience el proceso de interpretación y ejecución de los bytecodes (código objeto) debe realizar una serie de tareas para preparar el entorno en el que el programa se ejecutará. Este es el punto en el que se implementa la seguridad interna de Java.

Hay tres componentes en el proceso:

- El cargador de clases
- El verificador de ficheros de clases
- El gestor de seguridad

## El cargador de clases

Es el responsable de encontrar y cargar los bytecodes que definen las clases. Cada programa Java tiene como mínimo tres cargadores:
- El cargador de clases bootstrap que carga las clases del sistema (normalmente desde el fichero JAR rt.jar)
- El cargador de clases de extensión que carga una extensión estándar desde el directorio ```jre/lib/ext```
- El cargador de clases de la aplicación que localiza las clases y los ficheros **JAR/ZIP** de la ruta de acceso a las clases (según está establecido por la variable de entorno **CLASSPATH** o por la opción ```–classpath``` de la línea de comandos)

## El verificador de ficheros de clases

Se encarga de validar los bytecodes. Algunas de las comprobaciones que lleva a cabo son:

- Que las variables estén inicializadas antes de ser utilizadas
- Que las llamadas a un método coinciden con los tipos de referencias a objetos
- Que no se han infrigido las reglas para el acceso a los métodos y clases privados, etc.

## El gestor de seguridad

Es una clase que controla si está permitida una determinada operación.

Alguna de las operaciones que comprueban son las siguientes:

- Si el hilo actual puede cargar un subproceso
- Si puede acceder a un paquete específico
- Si puede acceder o modificar las propiedades del sistema
- Si puede leer desde o escribir en un fichero específico
- Si puede eliminar un fichero específico
- Si puede aceptar una conexión socket desde un host o número de puerto específico, etc.

Por defecto no se instala de forma automática ningún gestor de seguridad cuando se ejecuta una aplicación Java. En el siguiente ejemplo veremos la salida que produce el programa ejecutándolo sin gestor de seguridad y con gestor de seguridad. El programa muestra los valores de ciertas propiedades de sistema (usamos el método ```System.getProperty(propiedad)``` para mostrar los valores), la siguiente tabla describe alguna de las más importantes:

### Ejemplo1.java

```java
public class Ejemplo1 {
     
      public static void main(String[] args) {
           
            //propiedades de sistema en un array
            String t[] = {    "java.class.path", "java.home", "java.vendor",
                                   "java.version", "os.name", "os.version","user.dir",
                                   "user.home", "user.name"};
           
           
            System.setProperty ("java.security.policy", System.getProperty("user.dir") + "\\src\\_01SinConGestor\\Politica1.policy");
            System.setSecurityManager(new SecurityManager());
            for (int i = 0; i < t.length; i++) {
                 
                  System.out.print("Propiedad:" + t[i]);
                  try {
                        String s = System.getProperty(t[i]);     //valor de la propiedad
                        System.out.println("\t==> " + s);
                  } catch (Exception e) { System.err.println("\n\tEXcepción " + e.toString()); }
           
            }
      }
}
```

### Politica1.policy (para política de permisos)


```
grant {
      permission java.util.PropertyPermission "java.class.path", "read";
      permission java.util.PropertyPermission "java.home", "read";
      permission java.util.PropertyPermission "user.home", "read";
      permission java.util.PropertyPermission "user.name", "read";
      permission java.util.PropertyPermission "user.dir", "read";
      permission java.util.PropertyPermission "on.version", "read";
};
```

|PROPIEDAD|SIGNIFICADO|
|---|---|
|```file.separator```|Separador de directorios ("/" en UNIX y "\" en Windows)|
|```java.class.path```|Ruta usada para encontrar los directorios y ficheros JAR que contienen los archivos de clase|
|```java.home```|Directorio para JRE|
|```java.vendor```|Nombre del proveedor|
|```java.vendor.url```|URL del proveedor|
|```java.version```|Número de versión de JRE|
|```line.separator```|Fin de línea|
|```os.arch```|Arquitectura del sistema operativo|
|```os.name```|Nombre del sistema operativo|
|```os.version```|Versión del sistema operativo|
|```path.separator```|Caracter separador usado en ```java.class.path``` (En Unix es : mientras quwe en Windows es ;)|
|```user.dir```|Directorio en el que se está ejecutando el programa Java|
|```user.home```|Directorio por defecto del usuario|
|```user.name```|Nombre del usuario|

## APIs Java para seguridad

**JCA** (_Java Cryptography Architecture, Arquitectura Criptográfica de Java_). Es un marco de trabaj para acceder y desarrollar funciones criptográficas en la plataforma Java. Proporciona la infraestructura para la ejecución de los principales servicios de cifrado, incluyendo:

- Firmas digitales
- Resúmenes de mensajes (hash)
- Certificados y validación de certificados
- Encriptación (cifrado de bloques, cifrado simétrico y asimétrico)
- Generación y gestión de claves y generación segura de números aleatorios

**JSSE** (_Java Secure Extension, Extensión de Sockets Seguros Java_). Es un conjunto de paquetes Java provistos para la comunicación segura en Internet. Implementa una versión Java de los protocolos SSL y TLS. Incluye funcionalidades como:

- Cifrado de datos
- Autenticación del servidor
- Integridad de mensajes
- Autenticación del cliente

**JAAS** (_Java Authentication and Authorization Service, Servicio de Autenticación y Autorización de Java_). Es un interfaz que permite a las aplicaciones Java acceder a servicios de control de autenticación y acceso. Puede usarse con dos fines:

- La autenticación de usuarios para conocer quién está ejecutando código Java.
- La autorización de usuarios para garantizar que quién lo ejecuta tiene los permisos necesarios para hacerlo.

## Ficheros de política en Java

### Localización

El fichero de políticas predeterminado **java.policy**, está localizado en los siguientes directorios.

- Sistemas operativos Windows: ```java.home\lib\security\java.policy```
- Sistemas UNIX: ```java.home/lib/security/java.policy```

Donde ```java.home``` representa el valor de la propiedad **java.home**, que especifica el directorio en el que se instaló el **JRE**.

El valor por defecto es tener un solo fichero de políticas en todo el sistema y un fchero de políticas en el directorio ```home``` (dado por la variable **user.home**) del usuario (este tiene un punto delante).

En el fichero **java.security** localizado en la carpeta ```java.home\lib\security\``` se encuentran las localizaciones de estos ficheros:

```properties
policy.url.1=file:${java.home}/lib/security/java.policy
policy.url.2=file:${user.home}/.java.policy
```

### Entrada grant

Un fichero de políticas especifica qué permisos están disponibles para el código de varias fuentes.

Se utiliza para conceder permisos del sistema.

Contiene una secuencia de entradas **grant**, cada una de ellas tiene una o más entradas, el formato básico es el siguiente:

```bash
grant codeBase "URL"{
      permission Nombre_clase "Nombre_destino", "Acción";
      permission Nombre_clase "Nombre_destino", "Acción";
}
```


**¡Salud y coding!**