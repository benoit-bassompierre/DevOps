# DevOps
Formation DevOps

1-1 For which reason is it better to run the container with a flag -e to give the environment variables rather than put them directly in the Dockerfile?
Because by doing this, our container will allow to work with several variables and not only usr and pwd. Maybe also for safety reasons but putting personnal information in the environment variable is neither safe.

1-2 Why do we need a volume to be attached to our postgres container?
We need a volume so save our data even if we have to recreate our container.

1-3 Document your database container essentials: commands and Dockerfile.
image: postgres:14.1-alpine
variables : POSTGRES_DB=db   POSTGRES_USER=usr 

1-4 Why do we need a multistage build? And explain each step of this dockerfile.
Because by doing this, we avoid shipping build tools like Maven in the final image, which keeps it smaller and more secure. Also, the build is isolated from the runtime, which reduces risks and makes the image cleaner.

FROM eclipse-temurin:21-jdk-alpine AS myapp-build: starts a build container with JDK and labels it "myapp-build".
ENV MYAPP_HOME=/opt/myapp: sets a path variable for app files.
WORKDIR $MYAPP_HOME: sets the working directory.
RUN apk add --no-cache maven: installs Maven to build the app.
COPY pom.xml . and COPY src ./src: copies project files into the container.
RUN mvn package -DskipTests: builds the JAR without running tests.

Second stage uses a lighter image:
FROM eclipse-temurin:21-jre-alpine: only has the JRE to run the app.
Same ENV and WORKDIR to keep consistency.
COPY --from=myapp-build ...: copies the built JAR from the first stage.
ENTRYPOINT ...: defines how the app starts.

1-5 Why do we need a reverse proxy?
We need a reverse proxy to allow the access to all the containers with only one URL (the httpd's one). The reverse proxy will redirect every communication to the right container.

1-6 Why is docker-compose so important?
Docker compose allows to create all the containers with a template.

1-7 Document docker-compose most important commands.
docker-compose up starts all services as defined.
docker-compose down stops and removes containers, networks, and volumes.
docker-compose build builds images for all services.
docker-compose logs shows logs from running services.

1-8 Document your docker-compose file.
This docker-compose sets up a PostgreSQL database, a backend API, and an HTTP server, all on the same custom network. The database uses a volume for persistent data, and the API connects to it using environment variables.

1-9 Document your publication commands and published images in dockerhub.
docker tag my-database USERNAME/my-database:1.0
docker push USERNAME/my-database:1.0 
One image for each container in the docker-compose.yaml file:
benoitbassompierre/tp-devops-httpd
benoitbassompierre/tp-devops-database
benoitbassompierre/tp-devops-simple-api

1-10 Why do we put our images into an online repo?
Because we will allow any VM/other device to use them freely

2-1 What are testcontainers?
Testcontainers are Java libraries that let you run lightweight, throwaway containers for testing purposes. They will generate fictive data to test the container.

2-2 For what purpose do we need to use secured variables ?
To avoid hardcoding them

2-3 Why did we put needs: build-and-test-backend on this job? Maybe try without this and you will see!
Because before building and pushing the image on Dockerhub, we will need to check that the backend works well

2-4 For what purpose do we need to push docker images?
Because by pushing images, we can deploy our app on other machines or servers without rebuilding. 


The next steps are doing with on this repo started by forking the correction : https://github.com/benoit-bassompierre/tp-devops-correction-docker










