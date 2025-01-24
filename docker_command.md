## List of Docker Commands and Their Uses

### Docker Version

- **Command**: `docker version`
- **Usage**: Check the installed Docker version for both client and server.

### Docker Image

#### Viewing Image List
- **Command**: `docker image ls`
- **Usage**: Display a list of images available on the system.

#### Downloading an Image
- **Command**: `docker image pull imagename:tag`
- **Example**: `docker image pull redis:latest`
- **Usage**: Download an image from the Docker registry.

#### Deleting an Image
- **Command**: `docker image rm imagename:tag`
- **Example**: `docker image rm redis:latest`
- **Usage**: Delete an image from the system.

### Docker Container

#### Viewing All Containers
- **Command**: `docker container ls -a`
- **Usage**: Display all containers, including stopped ones.

#### Viewing Running Containers
- **Command**: `docker container ls`
- **Usage**: Display containers that are currently running.

#### Creating a New Container
- **Command**: `docker container create --name containername imagename:tag`
- **Example**: `docker container create --name rediscontainer redis:latest`
- **Usage**: Create a new container from an image.

#### Starting a Container
- **Command**: `docker container start containerId/containername`
- **Example**: `docker container start redis1`
- **Usage**: Start a previously created container.

#### Stopping a Container
- **Command**: `docker container stop containerId/containername`
- **Example**: `docker container stop redis1`
- **Usage**: Stop a running container.

#### Deleting a Container
- **Command**: `docker container rm containerId/containername`
- **Example**: `docker container rm redis1`
- **Usage**: Remove a stopped container.

### Docker Logs

#### Viewing Container Logs
- **Command**: `docker container logs containerId/containername`
- **Example**: `docker container logs redis1`
- **Usage**: Display logs from a container.

#### Viewing Logs in Real-Time
- **Command**: `docker container logs -f containerId/containername`
- **Example**: `docker container logs -f redis1`
- **Usage**: Display logs in real-time.

### Docker Exec

#### Running Commands Inside a Container
- **Command**: `docker container exec -i -t containerId/containername /bin/bash`
- **Example**: `docker container exec -i -t redis1 /bin/bash`
- **Usage**: Run commands inside a container, such as opening a shell session.

### Port Forwarding

#### Exposing Ports for a Container
- **Command**: `docker container create --name containername --publish laptopport:containerport image:tag`
- **Example**: `docker container create --name nginx1 --publish 8080:80 nginx:latest`
- **Usage**: Map a host port to a container port.

### Environment Variables

#### Adding Environment Variables
- **Command**: `docker container create --name containername --env KEY="value" --env KEY2="value" image:tag`
- **Example**: `docker container create --name mongo1 --publish 27017:27017 --env MONGO_INITDB_ROOT_USERNAME=fin --env MONGO_INITDB_ROOT_PASSWORD=fin mongo:latest`
- **Usage**: Add environment variables to a container for application configuration.

### Container Stats

- **Command**: `docker container stats`
- **Usage**: Display resource usage like CPU and memory for each container. Useful for monitoring excessive resource consumption.

### Container Resource Limit

#### Limiting Container Resource Usage
- **Command**: `docker container create --name containername --publish port1:port2 --memory memorylimit --cpus cpulimit imagename:tag`
- **Example**: `docker container create --name smallnginx --publish 8081:80 --memory 100m --cpus 0.5 nginx:latest`
- **Description**:
    - `--memory`: Set memory usage limit. Example: `100m` for 100 MB, `1g` for 1 GB.
    - `--cpus`: Set CPU usage limit. Example: `0.5` for half a CPU core.

### Bind Mounts

#### Using Bind Mounts for File Sharing
- **Command**: `docker container create --name containername --mount "type=bind,source=hostfolder,destination=containerfolder,readonly" imagename:tag`
- **Example**: `docker container create --name mongodata --mount "type=bind,source=/Users/khannedy/mongo-data,destination=/data/db" --publish 27018:27017 --env MONGO_INITDB_ROOT_USERNAME=fin --env MONGO_INITDB_ROOT_PASSWORD=fin mongo:latest`
- **Description**:
    - `type`: Mount type, e.g., `bind` or `volume`.
    - `source`: File or folder path on the host system.
    - `destination`: File or folder path inside the container.
    - `readonly`: Optional flag to make the file/folder read-only.

### Docker Volume

#### Advantages of Docker Volume
- Docker volumes are recommended over bind mounts because:
  - They support direct manipulation (creating, viewing, and deleting volumes).
  - Volume storage is managed directly by Docker, unlike bind mounts which use the host's filesystem.

#### Viewing Volume List
- **Command**: `docker volume ls`
- **Usage**: Display the list of volumes in Docker.

#### Creating a New Volume
- **Command**: `docker volume create volumename`
- **Example**: `docker volume create redisvolume`
- **Usage**: Create a new volume.

#### Deleting a Volume
- **Command**: `docker volume rm volumename`
- **Example**: `docker volume rm redisvolume`
- **Usage**: Delete a volume that is not being used by any container.

#### Container Volume (Attaching Volume to Container)
- **Command**: `docker container create --name containername --mount "type=volume,source=volumename,destination=containerfolder,readonly" image:tag`
- **Example**: `docker container create --name mongocontainer --mount "type=volume,source=mongovolume,destination=/data/db" --publish 27018:27017 --env MONGO_INITDB_ROOT_USERNAME=fin --env MONGO_INITDB_ROOT_PASSWORD=fin mongo:latest`
- **Usage**: Attach a volume to a container to ensure data persistence even if the container is deleted.

#### Backup Volume
- **Backup Steps**:
  1. Stop the container using the volume you want to back up:  
     `docker container stop containername`.
  2. Create a container with two mounts: one bind mount for the host folder and one volume for the data:  
     `docker container create --name backupcontainer --mount "type=bind,source=/backup,destination=/backup" --mount "type=volume,source=volumename,destination=/data" image:tag`.
  3. Start the container and perform the backup with the command:  
     `tar cvf /backup/backup.tar.gz /data`.

#### Restore Volume
- **Restore Steps**:
  1. Create a new volume for the restore location:  
     `docker volume create volumename`.
  2. Create a container with two mounts: a bind mount for the backup file and a volume for restoration:  
     `docker container create --name restorecontainer --mount "type=bind,source=/backup,destination=/backup" --mount "type=volume,source=volumename,destination=/data" image:tag`.
  3. Start the container and restore the backup:  
     `tar xvf /backup/backup.tar.gz --strip 1`.

### Docker Network

#### Viewing Networks
- **Command**: `docker network ls`
- **Usage**: Display the list of available networks.

#### Creating a New Network
- **Command**: `docker network create --driver drivername networkname`
- **Example**: `docker network create --driver bridge mynetwork`
- **Usage**: Create a new network with a specific driver (default: bridge).

#### Deleting a Network
- **Command**: `docker network rm networkname`
- **Example**: `docker network rm mynetwork`
- **Usage**: Remove a network that is not used by any container.
