TP_1

QUESTION 1:

For which reason is it better to run the container with a flag -e to give the environment variables rather than put them directly in the Dockerfile?

Using the -e flag at runtime is preferred because it keeps sensitive data out of the image, allows the same image to be reused across different environments, and ensures a clean separation between configuration and application logic.

QUESTION 2:

Why do we need a volume to be attached to our postgres container?

We need to attach a volume to the Postgres container to ensure that data persists even if the container is stopped, removed, or recreated.

QUESTION 3:

Document your database container essentials: commands and Dockerfile.

Build image : docker build -t nruchaud/postgredb .

Run container adminer : docker run -p "8090:8080" --net=app-network --name=adminer -d adminer                     

Run container database : docker run --env-file .env -p 8888:5000 --name postgredb --network app-network -v "C:\Users\natha\OneDrive - Fondation EPF\Documents\Cours\DevOps\Dev_Ops\TP1\database\data_tp1:/var/lib/postgresql/data" nruchaud/postgredb

QUESTION 4:

Why do we need a multistage build? And explain each step of this dockerfile ?

A multistage build is used to separate the compilation and execution phases, optimizing both security and image size. In the first stage of the Dockerfile, based on eclipse-temurin:21-jdk-alpine, Maven is installed and the Java source code is compiled into a .jar file using mvn package. This stage contains all the tools needed for building but not for running the application. In the second stage, a lighter image (eclipse-temurin:21-jre-alpine) is used to run the compiled .jar file, without including unnecessary tools like the JDK or Maven. The .jar is copied from the build stage into the runtime container and executed with java -jar. This approach results in a smaller, cleaner, and more secure final image while maintaining full build automation.

QUESTION 5:

Why do we need a reverse proxy?

reverse proxy routes external requests to internal services. It's useful to:
- Expose only one public endpoint
- Hide backend details and ports
- Enable SSL termination
- Handle load balancing
- Simplify URL routing

QUESTION 6:

Why is docker-compose so important?

Docker Compose simplifies multi-container management by defining services, networks, and volumes in one file, enabling easy orchestration with a single command.

QUESTION 7:

Document docker-compose most important commands.

docker-compose up – Starts all services defined in the YAML file.
docker-compose up --build – Builds images before starting containers.
docker-compose down – Stops and removes all containers, networks, and volumes.
docker-compose build – Builds or rebuilds services without starting them.
docker-compose ps – Lists running services and their status.
docker-compose logs – Shows logs from all containers.
docker-compose exec <service> <cmd> – Runs a command inside a running container (like bash or sh).
docker-compose restart – Restarts one or all services.

QUESTION 8:

Document your docker-compose file.

DevOpsDB
Builds the image from ./database.
Image name: tp1-devopsdb.
Environment variables: loaded from .env in the database directory.
Data persistence: stores PostgreSQL data in a named volume (db-data).
Network: connected to net1 to communicate with the backend.
Restart policy: restarts up to 3 times on failure.

DevOpsBACK
Builds the image from ./backend.
Image name: tp1-devopsback.
Environment variables: loaded from .env in the backend directory.
Networks:
net1: to connect with the database.
net2: to connect with the frontend.
Depends on DevOpsDB (ensures DB starts first).
Restart policy: restarts up to 3 times on failure.

DevOpsFRONT
Builds the image from ./frontend.
Image name: tp1-devopsfront.
Ports: exposes port 80 to the host.
Network: connected to net2 to forward requests to the backend.
Depends on DevOpsBACK (ensures backend starts first).
Restart policy: restarts up to 3 times on failure.

Networks
net1: private network between backend and database.
net2: private network between backend and frontend.

Volumes
db-data: persists PostgreSQL data to avoid loss on container restart/removal.

EXERCICE 9:

Document your publication commands and published images in dockerhub

Database:
docker tag tp1-devopsdb nathanrchd/tp1-devopsdb:1.0
docker push nathanrchd/tp1-devopsdb:1.0

Backend: 
docker tag tp1-devopsback nathanrchd/tp1-devopsback:1.0
docker push nathanrchd/tp1-devopsback:1.0

Frontend:
docker tag tp1-devopsfront nathanrchd/tp1-devopsfront:1.0
docker push nathanrchd/tp1-devopsfront:1.0

EXERCICE 10:

Why do we put our images into an online repo?

We push images to an online repository like Docker Hub to share them easily, reuse them on other machines, and enable automated deployment in team projects or CI/CD pipelines.
