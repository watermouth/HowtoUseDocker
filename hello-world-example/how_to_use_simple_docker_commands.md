# how to use basic docker commands: a simple example using hello-world image

## docker commands

### 1. create image using Dockerfile

``` bash
    # create the image
    docker build .
    # list created images
    docker images
```

### 2. run image

``` bash
    # create container and run
    docker run hello-world
    # list living container processes
    docker ps # nothing
    # list container processes including dead or alive
    docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
98751c80e9c3        hello-world         "/hello"            51 seconds ago      Exited (0) 50 seconds ago                       romantic_panini
```

#### 2.1. start the container again

``` bash
    docker start romantic_panini # shows nothing in STDOUT/STDERR
    docker start -a romantic_panini # shows the hello-world message in STDOUT/STDERR
```

### 3. remove container

``` bash
    # remove container using container id shown by "docker ps -a"
    docker rm 98751c80e9c3
```

### 4. remove image

``` bash
    # There exists 2-ways
    docker image rm hello-world
    docker rmi hello-world
```

該当コンテナから作成されたコンテナが残っていると実行できない.
コンテナを事前に削除するか, --force をつけて実行すると削除できる.

## docker-compose commands

``` bash
Creating network "hello-world-example_default" with the default driver
Building hello
Step 1/1 : FROM hello-world
 ---> bf756fb1ae65
Successfully built bf756fb1ae65
Successfully tagged hello-world-example_hello:latest
WARNING: Image for service hello was built because it did not already exist. To rebuild this image you must use `docker-compose build` or `docker-compose up --build`.
Creating hello-world-example_hello_1 ... done
Attaching to hello-world-example_hello_1
hello_1  | 
hello_1  | Hello from Docker!
hello_1  | This message shows that your installation appears to be working correctly.
hello_1  | 
hello_1  | To generate this message, Docker took the following steps:
hello_1  |  1. The Docker client contacted the Docker daemon.
hello_1  |  2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
hello_1  |     (amd64)
hello_1  |  3. The Docker daemon created a new container from that image which runs the
hello_1  |     executable that produces the output you are currently reading.
hello_1  |  4. The Docker daemon streamed that output to the Docker client, which sent it
hello_1  |     to your terminal.
hello_1  | 
hello_1  | To try something more ambitious, you can run an Ubuntu container with:
hello_1  |  $ docker run -it ubuntu bash
hello_1  | 
hello_1  | Share images, automate workflows, and more with a free Docker ID:
hello_1  |  https://hub.docker.com/
hello_1  | 
hello_1  | For more examples and ideas, visit:
hello_1  |  https://docs.docker.com/get-started/
hello_1  | 
hello-world-example_hello_1 exited with code 0
```

新たにhello-world-example_hello という imageが作成されている. この名前は 

> 実行ディレクトリ名_docker-compose.ymlで指定したservice名

である.

``` bash

watermouth@DESKTOP-RSIV4E1:~/proj/docker_sandbox/hello-world-example$ docker ps -a
CONTAINER ID        IMAGE                       COMMAND             CREATED              STATUS                          PORTS               NAMES
6a10222a8c73        hello-world-example_hello   "/hello"            About a minute ago   Exited (0) About a minute ago                       hello-world-example_hello_1
98751c80e9c3        hello-world                 "/hello"            45 minutes ago       Exited (0) 5 minutes ago                            romantic_panini

```

コンテナ名の命名規則は...(後で書く).

引き続いてもう一度実行すると, buildは行われず(imageは作成されず), containerも作成されず, 作成済みのcontainerを起動する.
docker start -a を実行していることになる.

``` bash
watermouth@DESKTOP-RSIV4E1:~/proj/docker_sandbox/hello-world-example$ docker-compose up
Starting hello-world-example_hello_1 ... done
Attaching to hello-world-example_hello_1
hello_1  | 
hello_1  | Hello from Docker!
hello_1  | This message shows that your installation appears to be working correctly.
hello_1  | 
hello_1  | To generate this message, Docker took the following steps:
hello_1  |  1. The Docker client contacted the Docker daemon.
hello_1  |  2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
hello_1  |     (amd64)
hello_1  |  3. The Docker daemon created a new container from that image which runs the
hello_1  |     executable that produces the output you are currently reading.
hello_1  |  4. The Docker daemon streamed that output to the Docker client, which sent it
hello_1  |     to your terminal.
hello_1  | 
hello_1  | To try something more ambitious, you can run an Ubuntu container with:
hello_1  |  $ docker run -it ubuntu bash
hello_1  | 
hello_1  | Share images, automate workflows, and more with a free Docker ID:
hello_1  |  https://hub.docker.com/
hello_1  | 
hello_1  | For more examples and ideas, visit:
hello_1  |  https://docs.docker.com/get-started/
hello_1  | 
hello-world-example_hello_1 exited with code 0
```
