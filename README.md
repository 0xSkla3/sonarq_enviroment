# sonarq_enviroment
entorno de sonarqube listo para usar standalone

## Prerequisitos
Tener instalado docker y docker compose, mvn y jdk17 (esto donde se vaya a levantar la app, puede ser en un container aparte o en el host)

## Steps
Tiramos docker de docker compose para levantar una base de datos postgresql enlazada al sonar y el sonarqube, ambos en containers distintos y con una network que los enlaza a los contenedores de manera interna. Opcional se puede agregar el parametro `-d` para que haga un detach, esto nos permite poder cerrar la consola y que todo corra en background, pero si hay algun error no lo veremos.

```
docker-compose up
```

Una vez temgamos levantado todo correctamente clonamos el repositorio de la app a testear, en este caso usaremos el WebGoat, una aplicacion vulnerable en base java-springboot la cual esta desarrollada y mantenida por la organizacion OWASP.

Esta aplicacion es vulnerable de manera adrede para poder probar y entender las distintas vulnerabilidades que compone el OWASP TOP10

```
git clone https://github.com/WebGoat/WebGoat.git
```

Una vez clonado el repositorio, procedemos a tirar el escaneo con el siguiente comando, siendo `-Dsonar.projectKey` el valor de la key con el cual creamos el proyecto en la gui de sonar, `-Dsonar.login` valor del token que nos dara el acceso al sonar y `-Dsonar.host.url` seria la ip donde esta ubicada el sonar. Si la app esta de manera local, puede ser `localhost` o `127.0.0.1`. En mi caso Levante otro container, un ubuntu, para poder hacer las pruebas sin necesidad de dejar residuos en mi OS. Por lo tanto esta ip seria la que me provee la interfaz del `docker0`

```
mvn clean verify sonar:sonar -Dsonar.sources=src/main/java -Dsonar.test=src/test,src/it -Dsonar.projectKey=YYYYYYYY -Dsonar.host.url=http://<sonar-ip>:9000 -Dsonar.login=token_XXXXXXXXXXXXXXX -Dsonar.projectBaseDir=.
```