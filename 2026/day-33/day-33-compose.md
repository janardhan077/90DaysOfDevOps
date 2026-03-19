🔹 Task 1: Install & Verify

Docker Compose is a tool used to define and run multi-container Docker applications.

It is usually included with modern Docker installations (Docker CLI plugin).

Verification ensures:

Docker Compose is installed correctly

Compatible version is available

Compose uses YAML files (docker-compose.yml) to configure services.

🔹 Task 2: Your First Compose File (Nginx)

A Compose file defines services, networks, and volumes.

For a basic setup:

Create a project folder (e.g., compose-basics)

Define one service (Nginx)

Map container port to host port (e.g., 80 → 8080)

Key Concepts:

Service: A container definition (like Nginx)

Port Mapping: Exposes container to browser

docker compose up:

Builds (if needed)

Creates and starts container

docker compose down:

Stops and removes container + network

Result:

You can access Nginx in browser using host port.

🔹 Task 3: Two-Container Setup (WordPress + MySQL)
Architecture:

WordPress (Frontend)

MySQL (Database)

Key Concepts:
1. Multiple Services

Each container is defined as a service

Services communicate using service names

2. Default Network

Docker Compose automatically creates a network

All services join this network

No manual linking required

3. Service Communication

WordPress connects to MySQL using:

MySQL service name (acts like hostname)

4. Environment Variables

Used to configure:

Database name

Username/password

Host connection

5. Volumes (IMPORTANT 🔥)

MySQL uses a named volume

Ensures:

Data is not lost when container stops or is removed

Persistence Check:

Stop and remove containers

Restart again

If volume is used → data remains intact

Without volume → data is lost

🔹 Task 4: Important Compose Commands (Concepts)
1. Detached Mode

Runs containers in background

Terminal remains free

2. View Running Services

Lists active containers managed by Compose

3. Logs

Helps debug container behavior

Can view:

All services logs

Specific service logs

4. Stop vs Down

Stop:

Containers stopped

Still exist

Down:

Containers + networks removed

5. Rebuild

Required when:

Dockerfile changes

App code changes (if baked into image)

🔹 Task 5: Environment Variables
1. Inline Variables

Defined directly inside docker-compose.yml

Useful for small configs

2. .env File

External file to store variables

Keeps secrets/config separate

Automatically read by Compose

Benefits:

Cleaner YAML file

Easier configuration management

Reusable across environments

Verification:

Ensure containers receive correct values
Key Takeaways

Docker Compose simplifies multi-container setups

Service names = internal DNS

Volumes = data persistence (very important)

Default networking = no manual config needed

.env = clean and scalable configuration
   Key Takeaways

Docker Compose simplifies multi-container setups

Service names = internal DNS

Volumes = data persistence (very important)

Default networking = no manual config needed

.env = clean and scalable configuration

up and down control full lifecycle
up and down control full lifecycle
