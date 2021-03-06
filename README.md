# Docker-Basics
Basic information about Docker to get you started

## what is docker?
- Docker is a tool for running application in an isolated environment
- docker is similar to a virtual machine
- everything inside of docker runs in the same environment
- it works on every machine
- it became standard software
## benefits of using docker
- run containers in seconds
- uses fewer resources and disk space compared to a virtual machine
- less memory usage
- easier to deploy an application
- does not need a full OS
- easier to test
> ## But what does it do?
> docker has a lot of power and it can do a lot of great things but for a simple dev like me I can use it to quickly install software just to test things out

> **, for example,** I am a .Net Developer I have SQL Server installed on my machine but if I want to test my project with Mysql, instead of installing it the regular way I just can do it within a seconds with one line of command and start using it. and when I am finished testing I can just delete everything with ease of use.

## basic termonologies
1. **Images**: Docker image is a read-only template that contains a set of instructions for creating as many containers you want.
we can have multiple version for the same image which is also called snapshot.
it is somehow similar to OS's ISO images that you can create an instance from it.
1. **Container**: Container is a lightweight, standalone, executable package of software that includes everything needed to run an application. 
basically, it is the instance we can create from the image and start using it.
1. **environment variable**: some images require some kind of environment variables to set for example MySQL database requires a root password to be set and it is used like this `-e MYSQL_ROOT_PASSWORD=my-secret-pw` 
1. **Tags**: Tags are the versions available for the images.

## basic Commands

> Note:
> Docker daemon must be running before start typing commands.
> Docker daemon is the application you downloaded.

|command|description|
|---|---|
|`docker`|shows command you can use|
|`docker pull`|download images from [docker hub](https://hub.docker.com)(can be skipped)|
|`docker pull imgname:xxx`|download images from [docker hub](https://hub.docker.com)(can be skipped) with xxx as a tag|
|`docker images`|shows downloaded images|
|`docker run`|download image (if not downloaded already) and create a container and run it|
|`docker run -d`|download image (if not downloaded already) and create a container|
|`docker run --name`|download image (if not downloaded already) and create a container with given name|
|`docker run -p XXXX:XXXX`|download image (if not downloaded already) and create a container with givin Port|
|`docker run -e`|download image (if not downloaded already) and create a container with environment variables|
|`docker ps`|shows running containers|
|`docker ps -a`|shows all containers|
|`docker stop`|stop running containers|
|`docker start`|start stoped containers|
|`docker rm`|remove containers|
|`docker rm -f`|remove containers by force|
|`docker rmi`|remove images|
|`docker rmi -f`|remove images by force|
|`docker -v`|mount your hosts folder directory to contaniners directory|
|`docker run --volumes-from`|copy everything from old container into new container|
|`docker exec -it containerName bash`|this will execute container in interactive mode with bash|

## Commands in Action
- `docker` this command shows you what options are available to use.
for example `docker --version` shows the version and build-id
```powershell
Docker version 19.03.13, build 4484c46d9d
```
- `docker pull` docker pull is used to download images from docker hub so we can create containers from it. this step can be skipped because `docker run` command can create a container and download image with the same command.
for example, we are going to download Nginx like this `docker pull nginx ` and this will show after that
```powershell
Using default tag: latest
latest: Pulling from library/nginx
bb79b6b2107f: Already exists
111447d5894d: Downloading  10.45MB/26.49MB
111447d5894d: Pull complete
a95689b8e6cb: Pull complete
1a0022e444c2: Pull complete
32b7488a3833: Pull complete
Digest: sha256:ed7f815851b5299f616220a63edac69a4cc200e7f536a56e421988da82e44ed8
Status: Downloaded newer image for nginx:latest
docker.io/library/nginx:latest
```
you can see by default it add Latest as a tag

- `docker pull imgname: xxx` this command does the same as the command above but we can select the version we want, like this
` docker pull nginx:1.19` this will download Nginx image with version 1.19

- `docker images` this command shows all images that available on your machine.
if you just started it should be empty but if you have some images it shows something like this.
```powershell
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
nginx               1.19                f35646e83998        8 days ago          133MB
nginx               latest              f35646e83998        8 days ago          133MB
```

## let's see what those headers mean.
1. **REPOSITORY**: this is the name of the image we have on our machine.
2. **TAG**: tag is the Image Version
3. **IMAGE ID**: it is self-explanatory it is the id of the image.
4. **CREATED**: is the date when the image was created or the last update of the image which we got from [Docker Hub](https://hub.docker.com/).
5. **SIZE**: is the Image size.

- `docker run ` this command is used for creating a container from the image if you have the image it uses that but if you didn't then it pulls it down.
for example: `docker run nginx` this will create a container and give it a random name then start the container as well
if you look at your terminal you can see you can't type anything as long as the container is running
you can open another terminal but we can make the container run in the background
to exit this container just press `ctrl + C` this won't stop the container from running.

now to run the container in the background type this.
1. `docker run -d` the `-d` is a flag it will create a container and run the container in detached mode basically it runs in the background until you stop it or you exit the daemon app.
1. `docker run -name` the `--name` flag gives our container a name of our choice for example: `docker run --name myNginx nginx:latest`

1. `docker run -p XXXX:XXXX` the x is replaced with the port we want to use and the ports of the image. for example `docker run -p 8080:80 nginx` now we can use this port to see what happened, visit localhost:8080 and you see a welcome message from nginx.

1.  `docker run -e` the `-e` is environment variables which not all container require it, for example, MySQL does need it to set up the root password like this
`docker run  -e MYSQL_ROOT_PASSWORD=my-secret-pw  mysql` replace `my-secret-pw` with your own password

> ## now combine all those tags you just learn to make a better container like this `docker run --name myNginx -d -p 8080:80 nginx:latest`


- `docker ps` this command shows the running Containers but not the stopped containers.

- `docker ps -a` this command list out all containers running or stopped ones. for more info you can check [official docks](docs.docker.com/engine/reference/commandline/ps/)


- `docker stop ` if you type `docker ps` you can see all running containers, stop them just type `docker stop` and then add the first 3 characters from the id or containers name for example:
this is my running containers
```powerhell
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
3bc169ba2e69        nginx               "/docker-entrypoint.…"   15 minutes ago      Up 15 minutes       0.0.0.0:8080->80/tcp   upbeat_austin
46f720bc2851        nginx:latest        "/docker-entrypoint.…"   25 minutes ago      Up 25 minutes       80/tcp                 myNginx
```

to stop the container with id use this command ` docker stop 3bc` or you can use containers name like this `docker stop upbeat_austin`
-  `docker start` if you want to start the container you can use the same methods as above but replace `stop` with `start`
 
now let's see all our container with `docker ps -a` command and remove the previous container, now if both of them are stopped you see something like this
```powershell
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                     PORTS               NAMES
3bc169ba2e69        nginx               "/docker-entrypoint.…"   15 minutes ago      Exited (0) 5 minutes ago                       upbeat_austin
46f720bc2851        nginx:latest        "/docker-entrypoint.…"   25 minutes ago      Exited (0) 5 minutes ago                       myNginx
```
the ports are empty because none of them is running if they are running it shows some port number

- `docker rm` now let's remove the first container with this command. just like stopping or starting containers, we can select them with id or their names for example:
`docker rm 3bc` now the first container is deleted. if you got an error that means your container is still running stop the container or add `-f` to the command to remove it by force like this `docker rm 460 -f`
- `docker rmi` with this we can also remove images just like removing containers but if you have multiple version like I do you better specify the version as well like this `docker image rmi nginx:latest` it gives an error if there are instance containers of the image so you can remove the containers or use force
- `docker -v` `-v` command alow us to share data like files and folders between containers or containers and host for example, to share data between my folder in d:/nginxFolder/ and nginxs folder run this command after you changed your own directory in the terminal
this how my terminal look
`PS D:\NginxFolder>`
now add this command 
`PS D:\NginxFolder> docker run --name nginxWebsite -v ${pwd}:/usr/share/nginx/html -dp 8080:80 nginx` I am on wnindows and using powershell that why we use `${pwd}` if you are on mac or linux use `$(pwd)` instead.

now you can go to D:/nginxFolder and create index.html page and run it in the browser like this Localhost:8080/index.html
> ### Note: from the document there is `ro` in the end, :ro mean read only with :ro you can't add files from the container remove it for example:
change this
`${pwd}:/usr/share/nginx/html:ro`
to this 
`${pwd}:/usr/share/nginx/html`

- `docker run --volumes-from` if you want to create new container with everything from old container we use this command for example:
`docker run --name nginxWebsite-copy --volumes-from nginxWebsite -d -p 8081:80 nginx` now if you visit localhost:8081 everything should be the same

- `docker exec -it ` if you want to access bash inside the container we use this command. for example `docker exec -it nginxWebsite bash` it is useful when we want to run command using linx bash inside container. now you can navigate to `/usr/share/nginx/html` and start creating files and folders for example `touch about.html` will create file now if you go to d:nginxFolder in windows you can see about.html created.
