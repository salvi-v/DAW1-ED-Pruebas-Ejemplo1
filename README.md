# DAW1-ED-Pruebas-Ejemplo1
[BuildPassingTravisCI](https://travis-ci.org/salvi-v/DAW1-ED-Pruebas-Ejemplo1.svg?branch=master)

[![SonarCloud](https://sonarcloud.io/images/project_badges/sonarcloud-white.svg)](https://sonarcloud.io)

[![Build Status](https://travis-ci.org/jamj2000/DAW1-ED-Pruebas-Ejemplo1.svg?branch=master)](https://travis-ci.org/jamj2000/DAW1-ED-Pruebas-Ejemplo1)
[![codecov](https://codecov.io/gh/jamj2000/DAW1-ED-Pruebas-Ejemplo1/branch/master/graph/badge.svg)](https://codecov.io/gh/jamj2000/DAW1-ED-Pruebas-Ejemplo1)
[![Sonar](https://sonarcloud.io/api/project_badges/measure?project=miapp&metric=alert_status)](https://sonarcloud.io/organizations/jamj2000-github/projects)


![JDK 8](https://img.shields.io/badge/JDK-8-blue.svg)
![Gradle](https://img.shields.io/badge/gradle-4-blue.svg)
![JUnit 4](https://img.shields.io/badge/JUnit-4-blue.svg)

[![GitHub issues](https://img.shields.io/github/issues/jamj2000/DAW1-ED-Pruebas-Ejemplo1.svg)](https://github.com/jamj2000/DAW1-ED-Pruebas-Ejemplo1/issues) 
[![GitHub forks](https://img.shields.io/github/forks/jamj2000/DAW1-ED-Pruebas-Ejemplo1.svg)](https://github.com/jamj2000/DAW1-ED-Pruebas-Ejemplo1/network)
[![GitHub stars](https://img.shields.io/github/stars/jamj2000/DAW1-ED-Pruebas-Ejemplo1.svg)](https://github.com/jamj2000/DAW1-ED-Pruebas-Ejemplo1/stargazers)
[![GitHub license](https://img.shields.io/github/license/jamj2000/DAW1-ED-Pruebas-Ejemplo1.svg)](https://github.com/jamj2000/DAW1-ED-Pruebas-Ejemplo1/blob/master/LICENSE)
[![HitCount](http://hits.dwyl.com/jamj2000/DAW1-ED-Pruebas-Ejemplo1.svg)](http://hits.dwyl.com/jamj2000/DAW1-ED-Pruebas-Ejemplo1)


## Pruebas unitarias en **Java** con **JUnit4** (Gradle)

### Requisitos previos

Este proyecto se ha desarrollado en Netbeans con el sistema de construcción **Gradle**. Por tanto es necesario tener instalado el plugin para `Gradle` instalado en Netbeans.

Además, si queremos hacer cobertura de código, deberemos editar el archivo `build.gradle`. Debemos insertar las siguientes líneas:

```groovy
plugins {
  id 'jacoco'
  // ...
}

jacocoTestReport {
    reports {
        xml.enabled false
        csv.enabled false
        html.destination file("${buildDir}/jacocoHtml")
    }
}
```
Esto permitirá de **gradle** pueda generar informes de cobertura de código.


### Código a testear (pruebas de unidad)

El código de la aplicación lo componen 3 clases:

- [Main](src/main/java/Main.java)  (Clase principal)
- [Aritmética](src/main/java/Aritmetica.java)
- [Utilidades](src/main/java/Utilidades.java)

La clase Main es la que hace uso de los métodos definidos en Aritmética y Utilidades.

Dentro de **Aritmética** tenemos 4 métodos estáticos:
- `int suma            (int num1, int num2)`
- `int resta           (int num1, int num2)`
- `int multiplicacion  (int num1, int num2)`
- `double division     (int num1, int num2)`
 
Dentro de **Utilidades** tenemos 1 métodos estático:
- `int [] ordenar (int num1, int num2, int num3)`  (para ordenar un array de 3 enteros)


### Clases de prueba

Las clases de prueba son:

- [MainTest](src/test/java/MainTest.java)
- [AritméticaTest](src/test/java/AritmeticaTest.java)
- [UtilidadesTest](src/test/java/UtilidadesTest.java)

Se ha comprobado también el constructor por defecto de cada clase, con la finalidad de que el informe de cobertura fuera del 100%.


### Ejecución de pruebas

Para ejecutar las pruebas sobre el código, ejecutaremos en el terminal de texto:

```
gradle  test
```

### Informe de cobertura

Para generar el informe de cobertura ejecutamos en el terminal de texto:

```
gradle  jacocoTestReport
```

Este comando debe ejecutarse desde la misma carpeta en la cuál tenemos el archivo `build.gradle`.

Para ver el informe de cobertura generado en formato HTML, ejecutamos desde el terminal de texto:

```
firefox   build/jacocoHtml/index.html
```


## Integración continua

Para integración continua (***construcción y paso de tests***) se hara uso de **[travis-CI.org](https://travis-ci.org/jamj2000/DAW1-ED-Pruebas-Ejemplo1)** 

Es importante tener un archivo **`.travis.yml`** adecuado. Aquí tienes el utilizado en este proyecto:

- [.travis.yml](.travis.yml)

En este archivo indicamos la plataforma necesaria para nuestra aplicación y los comandos necesarios para lanzar los tests. 

**Ejemplo simplificado**

```yaml
language: java
jdk:
  - openjdk8
script:
  - gradle  test  run
```


**Ejemplo de integración con travis-ci.org**

![travis-ci](img/travis-ci.png)


## Análisis estático de código

Tenemos varias formas de realizar análisis estático de código:

- SonarQube en equipo local
- SonarQube en SonarCloud.io
- NetBeans mediante Fuente/Inspect
- NetBeans mediante plugin FindBugs (está obsoleto)

Es preferible usar la primera forma.

### Análisis estático de código con SonarQube en equipo local

Para realizar un análisis de la calidad del código (bugs, vulnerabilidades, *code smells* y demás) recomiendo usar **SonarQube** en el equipo local. Podemos lanzar nuestro propio servidor sonarqube local. Para ello haremos uso de un contenedor de **Docker**.

> NOTA: Es necesario tener instalado previamente el software para docker.
>   Puedes consultar como hacerlo en https://github.com/jamj2000/docker

Iniciamos el contenedor con el servidor sonarqube. 

```bash
docker  run  -d  -p 9000:9000  --name sonarqube  sonarqube:lts
```
Podemos ver si el servicio se ha iniciado correctamente con el comando:

```bash
docker  ps
```

> NOTA: Es posible que nos aparezca el siguiente mensaje:
>
>       SonarScanner will require Java 11+ to run starting in SonarQube 8.x
>
> Así que si disponemos de una versión de Java inferior a la 11, deberemos usar SonarQube versión inferior a 8.
> Por ejemplo para usar la version 7.1-alpine de SonarQube ejecutaremos:
>
>       docker  rm   -f  sonarqube    # eliminamos contenedor anterior
>       docker  run  -d  -p 9000:9000  --name sonarqube  sonarqube:7.1-alpine

El servicio será accesible a través del puerto 9000. Tardará unos minutitos en estar disponible.

Para eliminar construcciones previas, volver a construir, pasar tests y realizar análisis estático con sonarqube, ejecutamos desde la carpeta donde tenemos el archivo de construcción `build.gradle` el siguiente comando:

```bash
gradle  clean  sonarqube
```

Visitamos la URL `http://localhost:9000`. Puede tardar un tiempo en cargar.

![Análisis de calidad del código](img/sonarqube-local-1.png)

![Análisis de calidad del código](img/sonarqube-local-2.png)

Es aconsejable realizar `Log in`, puesto que así podremos realizar tareas de edición. 

Por defecto existe un usuario **`admin`** con contraseña **`admin`**. Son las credenciales que utilzaremos. En la ventana emergente que aparece pulsar en `Skip this tutorial`.

Ahora, si pulsamos sobre el nombre de la aplicación, podremos ver un resumen.

![Análisis de calidad del código](img/sonarqube-local-3.png)

Ahora mismo, los apartados que tienen especial interés para nosotros son los de **`Code Smells`** y **`Coverage`**. El primero nos indica las prácticas de codificación que no se ajustan del todo a las recomendaciones y el segundo nos muestra la cobertura de código conseguida con los tests diseñados.

![Análisis de calidad del código](img/sonarqube-local-4.png)

Hay 2 tipos de `code smell`. Los *`major`* y los *`minor`*, siendo los primeros los que pueden tener más relevancia.

En el apartado de `coverage` podemos comprobar el % de cobertura de nuestros tests.

![Análisis de calidad del código](img/sonarqube-local-5.png)

También es posible examinar archivo por archivo. La línea verde que aparece a la izquierda es la cobertura realizada.

Aquí también podemos gestionar nuestros `code smells`. 

![Análisis de calidad del código](img/sonarqube-local-6.png)


### Usando SonarQube en Sonarcloud.io

- [Sonarcloud.io](https://sonarcloud.io/organizations/jamj2000-github/projects)

Nos hemos registrado con nuestra cuenta de GitHub en https://sonarcloud.io, hemos generado un *token* y hemos añadido este proyecto. 

Al principio del archivo [**`build.gradle`**](build.gradle) debemos escribir las líneas:

```
plugins {  
  id 'org.sonarqube' version '2.6'
  // ...
}
```
Para realizar el análisis, ejecutamos localmente la sentencia:

```
./gradlew sonarqube \
  -Dsonar.organization=jamj2000-github \
  -Dsonar.host.url=https://sonarcloud.io \
  -Dsonar.login=<token>
```
> NOTA: Debemos sustituir *\<token\>* por el generado previamente.

![Análisis de calidad del código](img/sonarqube-sonarcloud.png)


### Análisis estático de código en Netbeans

Otra forma, mucho más limitada, de realizar *análisis estático de código* desde Netbeans es ir al menú a **`Fuente`** -> **`Inspect`**.

![Inspect](img/inspect1.0.png)

En **`Configuration`** seleccionamos `Netbeans Java Hits`.

Y luego pulsamos en el botón `Inspect`.

![Análisis estático del código](img/Inspect-Netbeans-Java-Hints.png)

Se realizará una inspección muy básica de todos los proyectos que tenemos abiertos. Seleccionando nuestro proyecto, podemos inspeccionar los distintos archivos, tanto los que están en el *paquete de fuentes* como los que están en *paquete de tests*.

Por ejemplo, a continuación se muestra la clase `AritmeticaTest.java` donde se muestra que hay una sentencia `import` que no se usa. La mayoría de los avisos se refieren a la falta de documentación de Java (javadoc).

![Análisis estático del código](img/Inspect-Netbeans-Java-Hints-2.png)

Cómo puedes ver es una herramienta bastante limitada. Por tanto, lo más aconsejable es usar **Sonarqube** para este fin, que es una herramienta mucho más potente.


### Análisis estático de código con FindBugs en Netbeans

**ATENCIÓN: FindBugs es un proyecto que, al parecer, está más muerto que vivo. El plugin para Netbeans 8.2 no está disponible.**

**El texto que se muestra a continuación será eliminado en un futuro próximo. Disponible aún con fines históricos.**  


Una forma más sencilla de realizar análisis estático de código es utilizar el plugin **FindBugs** de Netbeans.

Deberemos primeramente instalar dicho plugin en el caso de no tenerlo ya instalado.

Para ello seguimos los siguientes pasos: **Herramientas -> Plugins**

![Herramientas -> Plugins](img/findbugs1.png)

En la pestaña **Plugins disponibles** buscamos *findbugs*

![Buscando plugin FindBugs](img/findbugs2.png)

Si no lo tenemos instalado, nos aparecerá en la parte izquierda para instalarlo. Lo marcamos y pulsamos en instalar. Después seguimos el asistente.

![Pulsar en siguiente](img/findbugs3.png)

Una vez instalado el plugin, ya podremos hacer uso de él para realizar **análisis estático de código**.

Para ello pulsamos en **Fuente -> Inspect**.

![](img/inspect1.0.png)

En **Scope** podemos escoger entre cuatro ámbitos, de más general a más específico:

- Open Projects
- Current Project
- Current Package
- Current File

![Scope](img/inspect1.1.png)

En **Configuration** podemos escoger entre distintas opciones. Seleccionaremos **All Analyzers**.

![All Analizers](img/inspect1.2.png)

A continuación se muestra Netbeans con la pestaña **Inspector** para la clase `AritmeticaTest`.

![Clase AritmeticaTest](img/inspect2.png)

A continuación se muestra Netbeans con la pestaña **Inspector** para todos los proyectos abiertos.

![Todos los proyectos abiertos](img/inspect3.png)

