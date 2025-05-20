# DevOps
Formation DevOps

Questions of the TP :
1-1 For which reason is it better to run the container with a flag -e to give the environment variables rather than put them directly in the Dockerfile?

For obvious safety reasons (password must not be visible in the dockerfile used to create the image) and for adaptability (being able to modify the password, username...)

1-2 Why do we need a volume to be attached to our PostgreSQL container?
Data persists even if the container is deleted

1-3 Document your database container essentials: commands and Dockerfile.
Network : app-network containing two containers : db and adminer

1-4 Why do we need a multistage build? And explain each step of this dockerfile.
We need the multistage build to separate the compilation and the execution of the code (jdk and jre)

1-5 Why do we need a reverse proxy?
A reverse proxy allow us to redirect the http requests to the containers. We use it to be able to acces all the containers of our network with only one URL: the reverse prroxy will redirect the request to the right container.
