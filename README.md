# RESTful Web Service Implementation with Docker

This project is part of assignment at Pace University.

The project uses Maven build and is implemented using Spring Boot.

The web service contains four GET routes:
<ul>
  <li>One that displays a collection of records (List all tvshows)</li>
  <li>One that displays a single record that corresponds to an ID (List a particular tvshow)</li>
  <li>One that displays a collection of records for a given entity (List of cast for individual tvshow)</li>
  <li>One that displays a single record from a collection of a given entity (List a particular actor from the cast for individual tvshow)</li>
</ul>

## Source Data
I have used a static JSON which can be found in "/src/main/resources/data.json" as source for data. 

## Get Requests available 
List all tvshows: localhost:8080  
List a particular tvshow: localhost:8080localhost:8080/showId  
List of cast for individual tvshow: localhost:8080/id/cast  
List a particular cast for individual tvshow: localhost:8080/showId/cast/castId  

### Follow the below steps to create a local environment and run this Spring Boot Application
## Step 1:
Download and unzip this source repository.

## Step 2:
Go to terminal or command prompt and change the directory to where you have downloaded and unziped the project. 
Type in the following:
#### `./mvnw package`
This command is used to execute all Maven phases until the package phase. It does the job of compiling, verifying and building the project.
It builds the jar file and places it in the target directory under the root project folder.

## Step 3:
Containerize the project:
Docker has a simple "Dockerfile" file format that it uses to specify the layers of an image. 
Dockerfile is included in this project and below is how I have created the Dockerfile:  
FROM openjdk:8-jdk-alpine  
VOLUME /tmp  
ARG DEPENDENCY=target/dependency  
COPY ${DEPENDENCY}/BOOT-INF/lib /app/lib  
COPY ${DEPENDENCY}/META-INF /app/META-INF  
COPY ${DEPENDENCY}/BOOT-INF/classes /app  
ENTRYPOINT ["java","-cp","app:app/lib/*","com.info.shows.SpringShowsCastApplication"]  
EXPOSE 8080

This Dockerfile has a DEPENDENCY parameter pointing to a directory. 
So from the terminal or command prompt type in the following:
#### `mkdir -p target/dependency && (cd target/dependency; jar -xf ../*.jar)`
This command will generate the following folders in the target/dependency folder
BOOT-INF/classes - with the application classes
BOOT-INF/lib - with the dependency jars
META-INF

## Step 4:
Now we have all the setup we need to build the docker image. To build the image you can use the Docker command line. 
#### `docker build -t springproj/DockerSpringWebservice .`
-t -- tags the imagename "DockerSpringWebservice", you can optionally f=give a tag name after the imagename and a : (DockerSpringWebservice:v1.0).  
If you dont provide a tagname, docker by defuault assigns latest to the tagname.'  
Tagnames are useful when you have to version your build images.  
You can check the image built by typing the following on your terminal/cmd:
#### `docker images`

## Step 5:
Now you can run the docker image that is built using the following docker run command:
#### `docker run -p 8080:8080 DockerSpringWebservice`
-p - publishes a container’s port(s) to the host  
Now go to the browser and type localhost:8080 and it will show you a JSON with the list of tvshows and their cast.  
To see the details of individual tvshows: localhost:8080/143 or localhost:8080/329 or localhost:8080/72  
To see the details of cast for individual tvshows: localhost:8080/143/cast or localhost:8080/329/cast or localhost:8080/4123009/cast  
To see the details of a particular cast for individual tvshows: localhost:8080/143/cast/20942 or localhost:8080/143/cast/27669 or localhost:8080/329/cast/67991 or localhost:8080/72/cast/5517 or localhost:8080/72/cast/5368 
