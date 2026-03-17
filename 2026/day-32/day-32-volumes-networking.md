Day 32 – Docker Volumes & Networking Notes
Containers are Ephemeral

Docker containers are ephemeral, meaning the data stored inside a container is temporary. When a container is stopped and removed, the data stored in that container is also removed.

Observation

A database container was created and some data was inserted.

After stopping and deleting the container, a new container was started.

Result
The previously created data was not available in the new container.

Reason
The data was stored inside the container’s writable layer, which is deleted when the container is removed.

Docker Volumes (Persistent Storage)

Docker volumes are used to store data outside the container’s filesystem. This allows data to persist even if the container is deleted or recreated.

Observation

A named volume was attached to the database container.

Data was inserted into the database.

The container was removed and recreated using the same volume.

Result
The data remained available because it was stored in the Docker volume, not inside the container.

Conclusion
Volumes provide persistent storage for containers, especially useful for databases and applications that require long-term data storage.

Bind Mounts

Bind mounts allow a directory from the host machine to be mounted directly inside a container.

Observation

A folder containing a web page file was created on the host machine.

The folder was mounted inside an Nginx container.

When the file was modified on the host machine, the changes appeared immediately inside the container.

Conclusion
Bind mounts are useful for development environments, where files need to be edited on the host system and instantly reflected inside the container.

Named Volume vs Bind Mount

Named Volume

Managed by Docker

Stored in Docker’s internal storage location

Commonly used for persistent data like databases

Bind Mount

Managed by the host operating system

Uses a specific directory from the host machine

Commonly used for development files or configuration files

Docker Networking

Docker provides networking features that allow containers to communicate with each other.

Default Bridge Network

When containers run on the default bridge network:

Containers can communicate using IP addresses

Container names are not automatically resolved

This means containers need to know the IP address of another container to communicate.

Custom Bridge Network

A custom bridge network improves container communication.

Observation

Two containers were connected to the same custom network.

They were able to communicate using container names instead of IP addresses.

Reason
Custom bridge networks include a built-in DNS service, which allows containers to resolve each other by name.

Key Learnings

Containers are ephemeral, so their data is lost when they are removed.

Docker volumes provide persistent storage for containers.

Bind mounts connect host directories directly to containers.

The default bridge network allows communication using IP addresses.

Custom bridge networks allow containers to communicate using container names through built-in DNS.
