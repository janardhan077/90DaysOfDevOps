Day 29 – Docker Basics Notes

1. Docker Containers

Containers allow applications to run in isolated environments.

They are lightweight and start faster than virtual machines.

2. Running a Container

docker run hello-world

This command pulls the image and runs a test container.

3. Running Container in Detached Mode

docker run -d ubuntu

-d runs the container in the background.

4. Interactive Container

docker run -it ubuntu /bin/bash

-i → interactive mode

-t → terminal access

5. List Running Containers

docker ps

Shows only running containers.

6. List All Containers

docker ps -a

Shows running and stopped containers.

7. Execute Command in Running Container

docker exec -it <container_id> /bin/bash

Runs commands inside a running container.
 
8. Remove a Container


docker rm <container_id>

Deletes a stopped container. <img width="1921" height="1080" alt="Screenshot from 2026-03-13 22-14-20" src="https://github.com/user-attachments/assets/d4fff26c-4175-4065-836f-698ea9d93506" />
<img width="1921" height="1080" alt="Screenshot from 2026-03-13 22-14-15" src="https://github.com/user-attachments/assets/ac683469-e6ec-4417-81e2-fb8fd06ea08c" />
<img width="1921" height="1080" alt="Screenshot from 2026-03-13 22-14-09" src="https://github.com/user-attachments/assets/0121eb41-7186-4f44-86ec-f5008c0331de" />
<img width="1921" height="1080" alt="Screenshot from 2026-03-13 21-49-16" src="https://github.com/user-attachments/assets/6fc5bb72-a487-4a0e-ab3b-8a348cd335d1" />

