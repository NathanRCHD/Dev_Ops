TP_1

QUESTION 1:

For which reason is it better to run the container with a flag -e to give the environment variables rather than put them directly in the Dockerfile?

Using the -e flag at runtime is preferred because it keeps sensitive data out of the image, allows the same image to be reused across different environments, and ensures a clean separation between configuration and application logic.

QUESTION 2:

Why do we need a volume to be attached to our postgres container?

We need to attach a volume to the Postgres container to ensure that data persists even if the container is stopped, removed, or recreated.

QUESTION 3:

Document your database container essentials: commands and Dockerfile.

Build image : docker build -t nruchaud/mysecondapp .

Run contener adminer : docker run -p "8090:8080" --net=app-network --name=adminer -d adminer                     

Run contener database : docker run --env-file .env -p 8888:5000 --name mysecondapp --network app-network -v "C:\Users\natha\OneDrive - Fondation EPF\Documents\Cours\DevOps\Dev_Ops\TP1\flask-app\data_tp1:/var/lib/postgresql/data" nruchaud/mysecondapp
