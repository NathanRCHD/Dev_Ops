
# TP_1 – Docker & Docker Compose

---

## Question 1

### *Why is it better to run the container with a flag `-e` to give the environment variables rather than put them directly in the Dockerfile?*

Using the `-e` flag at runtime is preferred because it:
- Keeps sensitive data out of the image,
- Allows reuse of the same image across environments,
- Ensures a clean separation between configuration and application logic.

---

## Question 2

### *Why do we need a volume to be attached to our postgres container?*

To persist data even if the container is stopped, removed, or recreated.

---

## Question 3

### *Document your database container essentials: commands and Dockerfile.*

#### Build the image
```bash
docker build -t nruchaud/postgredb .
```

#### Run Adminer
```bash
docker run -p "8090:8080" --net=app-network --name=adminer -d adminer
```

#### Run PostgreSQL
```bash
docker run --env-file .env -p 8888:5000 --name postgredb   --network app-network   -v "C:\Users\natha\OneDrive - Fondation EPF\Documents\Cours\DevOps\Dev_Ops\TP1\database\data_tp1:/var/lib/postgresql/data"   nruchaud/postgredb
```

---

## Question 4

### *Why do we need a multistage build?*

To **separate build and runtime**, producing smaller, cleaner, and more secure images.

#### 🔧 Steps:
1. **Stage 1 – Build**
   - Uses `eclipse-temurin:21-jdk-alpine`
   - Installs Maven and compiles `.jar`
2. **Stage 2 – Run**
   - Uses `eclipse-temurin:21-jre-alpine`
   - Copies `.jar` and runs it with `java -jar`

---

## Question 5

### *Why do we need a reverse proxy?*

To:
- Expose one public endpoint
- Hide internal ports
- Enable SSL
- Handle load balancing
- Simplify routing

---

## Question 6

### *Why is Docker Compose so important?*

It simplifies multi-container orchestration with a single command and a declarative YAML file.

---

## Question 7

### *Most important Docker Compose commands*

```bash
docker-compose up             # Start all services
docker-compose up --build    # Build before starting
docker-compose down          # Stop and remove services
docker-compose build         # Build services without starting
docker-compose ps            # List running services
docker-compose logs          # Show logs
docker-compose exec <svc> sh # Run command in container
docker-compose restart       # Restart services
```

---

## Question 8

### *Document your docker-compose file*

#### `DevOpsDB`
- Builds from `./database`, image: `tp1-devopsdb`
- Loads env from `.env`
- Uses volume `db-data` for persistence
- Network: `net1`
- Restart policy: `unless stopped`

#### `DevOpsBACK`
- Builds from `./backend`, image: `tp1-devopsback`
- Env from `.env`
- Networks: `net1`, `net2`
- Depends on: `DevOpsDB`
- Restart policy: `on-failure:3`

#### `DevOpsFRONT`
- Builds from `./frontend`, image: `tp1-devopsfront`
- Exposes port `80`
- Network: `net2`
- Depends on: `DevOpsBACK`
- Restart policy: `no`

#### Networks
```yaml
net1: backend <--> database
net2: backend <--> frontend
```

#### Volumes
```yaml
db-data: persistent PostgreSQL data
```

---

## Exercice 9

### *Docker Hub Publication Commands*

#### Database:
```bash
docker tag tp1-devopsdb nathanrchd/tp1-devopsdb:1.0
docker push nathanrchd/tp1-devopsdb:1.0
```

#### Backend:
```bash
docker tag tp1-devopsback nathanrchd/tp1-devopsback:1.0
docker push nathanrchd/tp1-devopsback:1.0
```

#### Frontend:
```bash
docker tag tp1-devopsfront nathanrchd/tp1-devopsfront:1.0
docker push nathanrchd/tp1-devopsfront:1.0
```

---

## Exercice 10

### *Why do we put our images into an online repo?*

To easily **share, reuse, and deploy** images across machines and teams, especially in CI/CD pipelines.

# TP_2


2-1 What are Testcontainers?
Testcontainers are Java libraries that let you spin up Docker containers during integration tests. They simulate real environments, like PostgreSQL, for realistic testing.

2-2 Why do we use secured variables?
To protect sensitive data like credentials or tokens from being exposed in logs, code, or public repositories.

📌 2-3 Why did we put needs: test-backend?
It ensures the image build job only runs if the backend tests pass. Without it, Docker images could be built even if your code is broken.

📌 2-4 Why do we push Docker images?
To deploy the app elsewhere (production, staging, team machines), and enable reproducible, versioned delivery of the application



# TP_3

2
Installs necessary system packages (apt-transport-https, curl, lsb-release, etc.)

Adds Docker’s official GPG key

Configures the Docker APT repository dynamically using the ansible_facts for the distribution release

Installs Docker (docker-ce)

Installs Python 3 and pip3

Sets up a virtual environment for Python packages

Installs the Docker SDK for Python inside the virtual environment

Ensures the Docker service is up and running