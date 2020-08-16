## To start docker-compose

- Run docker-compose to start app

```sh
$ cd project
$ docker-compose up --build
```

- Open browser

```sh
http://localhost:5001
```

| Command                 | Action                                                                                            |
| ----------------------- | ------------------------------------------------------------------------------------------------- |
| `docker-compose`        | every commands starts with this.                                                                  |
| `docker-compose build`  | buid images of mentioned services in docker-compose.yml file                                      |
| `docker-compose images` | lists images built using the current docker-compose file                                          |
| `docker-compose run`    | creates containers from services mentioned in docker-compose file. it can run a specific service. |

```sh
$ docker-compose run web
Starting 7001788f31a9_docker_database_1 ... done
 * Serving Flask app "app.py" (lazy loading)

Note: "web" depends on "database" which is the reason "database" started. Otherwise, "database" will not start.
```

| Command                  | Action                                                                                               |
| ------------------------ | ---------------------------------------------------------------------------------------------------- |
| `docker-compose up`      | Does `docker-compose build` and `docker-compose run` . Force to rebuild `docker-compose up --build`. |
| `docker-compose stop`    | Force to stop the running containers of the specified services in docker-compose file.               |
| `docker-compose rm`      | Removes the containers mentioned in docker-compose file                                              |
| `docker-compose start`   | Starts any stopped containers of the services.                                                       |
| `docker-compose restart` | Restarts all the containers of the services.                                                         |
| `docker-compose ps`      | Lists all the containers for servcies in the current docker-compose file.                            |
| `docker-compose down`    | Stops all the services and clean up the containers in docker-compose file.                           |
| `docker-compose logs`    | Prints all the logs created by all the servcies. Use `-f` flag to print real-time logs.              |

---

<br/>

## Explanation of docker-compose yaml file

| Command       | Action                                                                                                                                                     |
| ------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `version`     | Version of docker-compose file. Use the latest as it is backward compatiable.                                                                              |
| `services`    | Defines different containers                                                                                                                               |
| `web`         | This can be anything. Name of container created by docker-compose                                                                                          |
| `build`       | Specifies the location of Dockerfile                                                                                                                       |
| `ports`       | Map to expose the container ports to the host machine                                                                                                      |
| `volumes`     | Attaching code files directory to the containers.                                                                                                          |
| `links`       | Define extra aliases by which a service is reachable from another service. "database" is reachabel from "web" at the hostnames "database" and "backenddb". |
| `image`       | If there is no Dockerfile and want to run pre-built image, specify the image location.                                                                     |
| `environment` | Environmental variables need to be set up in the container.                                                                                                |
| `depends_on`  | Express dependencies between sevices. In the following "database" is started before "web".                                                                 |

```docker
version: "3.7"
services:
  web:
    # Path to dockerfile.
    # '.' represents the current directory in which
    # docker-compose.yml is present.
    build: .

    # Mapping of container port to host

    ports:
      - "5000:5000"
    # Mount volume
    volumes:
      - ".:/code"

    # Link database container to app container
    # for reachability.
    links:
      - "database:backenddb"
    depends_on:
      - database

  database:
    # image to fetch from docker hub
    image: mysql/mysql-server:5.7

    # Environment variables for startup script
    # container will use these variables
    # to start the container with these defined variables.
    environment:
      - "MYSQL_ROOT_PASSWORD=root"
      - "MYSQL_USER=testuser"
      - "MYSQL_PASSWORD=admin123"
      - "MYSQL_DATABASE=backend"
    # Mount init.sql file to automatically run
    # and create tables for us.
    # everything in docker-entrypoint-initdb.d folder
    # is executed as soon as container is up nd running.
    volumes:
      - "./db/init.sql:/docker-entrypoint-initdb.d/init.sql"
```
