## Docker Architecture

Docker uses a client-server architecture. The Docker client talks to the Docker daemon, which does the heavy lifting of building, running, and distributing your Docker containers. The Docker client and daemon can run on the same system, or you can connect a Docker client to a remote Docker daemon.

![docker architecture](https://docs.docker.com/engine/images/architecture.svg)

[Source: https://docs.docker.com/get-started/overview/]

---

## Install Docker

- Sign in and follow intruction

https://hub.docker.com/?overlay=onboarding

- To verify Docker works in your machine, run the following. If you see Docker pull hello-world and run it, Docker works fine in your system.

```sh
$ docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
0e03bdcc26d7: Pull complete
```

---

## Running APP with CLI

You can run the Flask app in project folder locally with CLI.
Verify it works first.

- Mac/Linux

```sh
$ python3 -m venv <venvname>
$ source <venvname>\bin\activate

$ cd project

$ pip install -r requirements.txt
$ export FLASK_APP=app.py
$ flask run
```

- Win

```sh
# set up venv here
> cd project
> pip install -r requirements.txt
> set FLASK_APP = "app.py"
> flask run
```

---

## Running APP in Docker

- Create "Dockerfile"

```sh
FROM python:3.9-rc-buster

# Setting up Docker environment
WORKDIR /code
# Export env variables.
ENV FLASK_APP app.py
ENV FLASK_RUN_HOST 0.0.0.0
###

#Copy requirements file from current directory to file in
#containers code directory we have just created.
COPY requirements.txt requirements.txt

#Run and install all required modules in container
RUN pip3 install -r requirements.txt

#Copy current directory files to containers code directory
COPY . .

#RUN app.
CMD ["flask", "run"]
```

- In the folder Dockerfile is create, run the following to build Docker image.

```sh
$ docker build -t flask_app:1.0 .
```

- Make sure Docker image is correctly created.

```sh
$ docker images
REPOSITORY TAG IMAGE ID CREATED SIZE
flask_app 1.0 62302a64901a 10 seconds ago 896MB
```

- Run Docker image.

```sh
$ docker run -p 5001:5000 flask_app:1.0
```

- Open your brower
  5001:5000 port mapping means port 5000 in Docker mapping to 5001 in host. You can choose any host port other than 5001.

```sh
http://localhost:5001
```

---

## Docker commands

| Command      | Action                                                                                    |
| ------------ | ----------------------------------------------------------------------------------------- |
| `FROM`       | Which base image to use?                                                                  |
| `WORKDIR`    | Working directory for Docker commands like CMD, RUN                                       |
| `ENV`        | Environmental variable required for app                                                   |
| `COPY`       | Copies file from source to destination &nbsp; `COPY <SOURCE> <DESTINATION>`               |
| `RUN`        | Execute any commands in a new layer on top of the current image.                          |
| `CMD`        | Default command user can override.                                                        |
| `ENTRYPOINT` | Similar to CMD. Preferred when you want to define a container with a specific executable. |

---

## Useful Access Commands

- List all running containers

```sh
$ docker ps
```

- Show all logs the app created so far

```sh
$ docker logs <CONTAINER ID>
```

- Print all logs and listen for new activity

```sh
$ docker logs -f <CONTAINER ID>
```

- Pipe logs to output file

```sh
$ docker logs <CONTAINER ID> > output.txt
```

---

## Troubleshooting in Docker

- Print all the services using the specified port

```sh
$ lsof -i TCP:<port_number>
```

- Print space used by Docker

```sh
$ docker system df
```

- Prompt to notify what it should remove

```sh
$ docker system prune
WARNING! This will remove:
  - all stopped containers
  - all networks not used by at least one container
  - all dangling images
  - all dangling build cache

Are you sure you want to continue? [y/N] y
```

- Get all configuration of a container, an image or a network

```sh
$ docker inspect <Container/Image/Network ID>
```

---

## Pushing an app image to Docker Hub (Registry)

- Log in to Docker Hub or Docker Registry, You already have generated username and password to download Docker CE.

```sh
$ docker login

Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: <username>
Password: <password>
Login Succeeded
```

- Generate a tagged image and push to Docker Hub (or Registry)

```sh
# generate a new tagged image to push to Docker Hub (or Registry)
$ docker tag flask_app:1.0 <username>/flask_app:1.0

# verify it new image tagged.
$ docker images
REPOSITORY                                 TAG                 IMAGE ID
flask_app                                  1.0                 3507ec2e1851
<username>/flask_app                       1.0                 3507ec2e1851

# push to Docker Hub (or Registry)
$ docker push <username>/flask_app:1.0
```

- Pull Docker image from Docker Hub (or Registry).

```sh
docker pull <username>/flask_app:1.0
```
