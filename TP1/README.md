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

