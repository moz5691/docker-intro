| Command                                 | Function                                                                                       |
| --------------------------------------- | ---------------------------------------------------------------------------------------------- |
| `docker ps`                             | shows all running containers                                                                   |
| `docker ps -a`                          | shows all available containers, both stopped and running                                       |
| `docker pull \<images-name>:\<version>` | pulls image from Docker registry                                                               |
| `docker run \<images-name>:\<version>`  | runs container from mentioned image                                                            |
| `docker exec`                           | executes a command in a running container                                                      |
| `docker inspect <contaier_name>`        | Provides all info about the container                                                          |
| `docker stop <container_name>`          | Stops the running container                                                                    |
| `docker kill <container_name>`          | Kills(stops) the container                                                                     | and removes the container from the system |
| `docker rmi`                            | Removes the provided image                                                                     |
| `docker images`                         | Lists all images on the system                                                                 |
| `docker exec`                           | Executes command in a Docker container                                                         |
| `docker system`                         | Gets the Docker system information such as memory usage and housekeeping stuff                 |
| `docker system prune`                   | This command will save you from getting the “No memory left” nightmare with production systems |
