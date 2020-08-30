# how to use basic docker commands: node-bulletin-board example

## docker commands

``` bash
    # setup
    git clone https://github.com/dockersamples/node-bulletin-board
    cd node-bulletin-board/bulletin-board-app
```

### 1. create image using Dockerfile

``` bash
    docker build --tag bulletinboard:1.0 .
    $ docker build --tag bulletinboard:1.0 .
    Sending build context to Docker daemon  45.57kB
    Step 1/7 : FROM node:current-slim
    current-slim: Pulling from library/node
    75cb2ebf3b3c: Pull complete 
    1c6bd282486b: Pull complete 
    0266fdd2bd16: Pull complete 
    96b25b214830: Pull complete 
    a4cd0deecb85: Pull complete 
    Digest: sha256:55e800b777d4e93a2b660dd4bde1e34d632f668941bc9c116e0ca7857c2ae6c9
    Status: Downloaded newer image for node:current-slim
    ---> 05f62d57259e
    Step 2/7 : WORKDIR /usr/src/app
    ---> Running in 214199c0b732
    Removing intermediate container 214199c0b732
    ---> da22d74c8fc3
    Step 3/7 : COPY package.json .
    ---> d43d5bf582f6
    Step 4/7 : RUN npm install
    ---> Running in 6fa06a269af3

    > ejs@2.7.4 postinstall /usr/src/app/node_modules/ejs
    > node ./postinstall.js

    Thank you for installing EJS: built with the Jake JavaScript build tool (https://jakejs.com/)

    npm notice created a lockfile as package-lock.json. You should commit this file.
    npm WARN vue-event-bulletin@1.0.0 No repository field.
    npm WARN The package morgan is included as both a dev and production dependency.

    added 91 packages from 168 contributors and audited 92 packages in 10.959s
    found 0 vulnerabilities

    Removing intermediate container 6fa06a269af3
    ---> b8c3b5220e9a
    Step 5/7 : EXPOSE 8080
    ---> Running in f2307b50f41f
    Removing intermediate container f2307b50f41f
    ---> b2cd42726d3d
    Step 6/7 : CMD [ "npm", "start" ]
    ---> Running in 127e91a2d9db
    Removing intermediate container 127e91a2d9db
    ---> 8f737cb9308d
    Step 7/7 : COPY . .
    ---> a9d8d2ffceb8
    Successfully built a9d8d2ffceb8
    Successfully tagged bulletinboard:1.0
```

bulletinboard:1.0 というrepository name:tag を持つimageが作成される.
node:current-slimというimageも作成されているが, これはDockerfileのFROMで指定したimageであり, 
downloadしただけである.

``` bash
    $ docker images
    REPOSITORY                  TAG                 IMAGE ID            CREATED              SIZE
    bulletinboard               1.0                 a9d8d2ffceb8        About a minute ago   183MB
    node                        current-slim        05f62d57259e        32 hours ago         167MB
```

### 2. run the image

``` bash
    $ docker run --publish 8000:8080 --detach --name bb bulletinboard:1.0
    $ docker ps -a
    CONTAINER ID        IMAGE                       COMMAND                  CREATED             STATUS                      PORTS                    NAMES
    a97acf34508d        bulletinboard:1.0           "docker-entrypoint.s…"   7 minutes ago       Up 7 minutes                0.0.0.0:8000->8080/tcp   bb
    c21ce91bda91        hello-world-example_hello   "/hello"                 26 minutes ago      Exited (0) 25 minutes ago                            hello-world-example_hello_1
    d6bdc5e1e1bf        hello-world                 "/hello"                 28 minutes ago      Exited (0) 27 minutes ago                            adoring_jones
    $ docker ps # bb is running
    CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
    a97acf34508d        bulletinboard:1.0   "docker-entrypoint.s…"   7 minutes ago       Up 7 minutes        0.0.0.0:8000->8080/tcp   bb
```

- --publish : host(dockerを動かしているもの)のポート番号:conainer内のport number hostのport番号に来るデータをcontainer の port 番号に転送する. forward traffic incoming on the host’s port 8000 to the container’s port 8080.
- --detach : back ground で実行する.
- --name : 指定した名前のコンテナ名にてimageを起動する.

web browser にて localhost:8000 にアクセスするとbulletinboard アプリを利用できる.
試しにデータを追加する.

``` 3. stop and run the container 

``` bash
    docker stop bb
    docker ps # bb is not running
    docker start bb
```

先ほど登録したデータが消えていることを確認できる.

``` 4. remove the container

``` bash
    docker rm --force bb
```

実行中のコンテナを直接削除する場合は--forceをつける.

### Appendix. Dockerfile explanation

``` Dockerfile
# Use the official image as a parent image.
FROM node:current-slim

# Set the working directory.
WORKDIR /usr/src/app

# Copy the file from your host to your current location, i.e. the image file system's /usr/src/app
COPY package.json .
# created /usr/src/app/package.json in the image file system.

# Run the command inside your image filesystem.
# execute npm install on /usr/src/app using /usr/src/app/package.json.
RUN npm install

# Add metadata to the image to describe which port the container is listening on at runtime.
EXPOSE 8080

# Run the specified command within the container.
CMD [ "npm", "start" ]

# Copy the rest of your app's source code from your host to your image filesystem.
COPY . .
```

## docker-compose

``` bash
    docker-compose up -d # execute background
```
