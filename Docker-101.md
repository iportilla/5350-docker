<h1 align="center" style="border-bottom: none;">🔎 Docker 101 </h1>
<h3 align="center">Docker is an open-source project that automates the deployment of software applications inside containers by providing an additional layer of abstraction and automation of OS-level virtualization on Linux..</h3>

Learn more with [Docker get started guide](https://docs.docker.com/get-started)

How is Docker different from a virtual machine?

<img width="914" height="434" alt="image" src="https://github.com/user-attachments/assets/a0a16786-68d1-4b42-9015-cdf0441830ca" />

How does Docker work?

<img width="959" height="495" alt="image" src="https://github.com/user-attachments/assets/41a02028-304a-4a8f-b764-c9cbabeb58d9" />


Tell me more:

<img width="782" height="537" alt="image" src="https://github.com/user-attachments/assets/c0010dad-a1fb-436d-8bb8-599c60fa9d65" />



### Flow

In this topic, you'll follow a series of hands-on exercises that demonstrate how to use containers for your applications. You'll start with the basics: creating and running your first Docker containers. By the end of the course, you'll get a brief introduction to running containers in production.


<h3>STAT-5350/4350</h3>
</p>

1. login to the development virtual machine VM hosted on Azure cloud

	```
    ssh azureuser@XX.XX.XX.XX
    ```


    
    *Make sure to post your **ssh public key** to the Slack channel used for this lesson
    
2. Navigate to your team subdirectory

	```
 	cd team-1
 	```

	or team-2,team-3, ...

4. Create & navigate to your own directory:

   ```

	mkdir userName
	
	cd userName

   ```
	
	For example:

	```
	mkdir ivanp
	
	cd ivanp

 	```
	
	
6. Clone Docker repository from github:

   ```
	git clone https://github.com/iportilla/5350-Docker.git
   ```
	
8. Change directory to the Docker directory:

   ```
	cd 5720-Docker/
   ```
   
10. Test your `docker` installation by running the following command:

	```
	docker run hello-world
 	```
	
	You will see:
	
	```
	Hello from Docker.
	This message shows that your installation appears to be working correctly.
	...
	
	```
	
### Hello World

7. Next, we are going to run a `Busybox` container on our system and get a taste of the `docker run` command. To get started, let's run the following in our terminal:.

	```
	docker pull busybox
	```
	You will see:
	
	```	
	Using default tag: latest
	latest: Pulling from library/busybox
	...
	```

	The `pull` command fetches the busybox image from the Docker registry and saves it to your system.
	You can use the `docker images` command to see a list of all images on your system:

	```
	docker images
 	```

 	You will see a list of docker images:
	```
 	REPOSITORY          TAG       IMAGE ID       CREATED         SIZE
	hello-world         latest    1b44b5a3e06a   2 months ago    10.1kB
	busybox             latest    08ef35a1c3f0   13 months ago   4.43MB
 	...
	```

9. Great! Let's now run a Docker container based on this image. Run the following command:

	```
 	docker run busybox echo "Hello World from busybox"
 
 	```

9. Let's run a terminal in the busybox container with:

	```
 	docker run -it busybox /bin/sh
 	```
 
 	Test you are running inside the container with:
	```
 	ls
 	```
 
 	You will see:
	```
	bin    etc    lib    proc   sys    usr
	dev    home   lib64  root   tmp    var
 	```
 
 	Exit the container with:
	```
 	exit
 	```

### Static Site

The first thing we're going to look at is how we can run a dead-simple static website. We're going to pull a Docker image from Docker Hub, run the container and see how easy it is to run a webserver.

1. Detached mode, run:
```
docker run -d -p 80:80 --name static-site prakhar1989/static-site
```

In the above command, `-d` will detach our terminal, `-P` will publish all exposed ports to `80:80` and finally `--name` corresponds to a name we want to give. Now we can see the ports by running the `docker port [CONTAINER]` command:

```
docker port static-site
```

You will see:
```
80/tcp -> 0.0.0.0:80
80/tcp -> [::]:80
```

Open your browser and navigate to:

`http://XX.XXX.XXX.XXX`

Using the IP address above.

Check running containers with:

```
docker ps
```

You will see:
```
CONTAINER ID   IMAGE                     COMMAND          CREATED              STATUS              PORTS                                          NAMES
451bcb5a77c6   prakhar1989/static-site   "./wrapper.sh"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp, [::]:80->80/tcp, 443/tcp   static-site
```

Stop running container with:

```
docker stop static-site
```

Prune stopped containers with:
```
docker container prune
```


## Docker Images

We have looked at images before, but in this section we'll dive deeper into what Docker images are and build our own image! Lastly, we'll also use that image to run our application locally and finally deploy on AWS to share it with our friends! Excited? Great! Let's get started.

Docker images are the basis of containers. In the previous example, we pulled the Busybox image from the registry and asked the Docker client to run a container based on that image. To see the list of images that are available locally, use the docker images command.

```
docker images
```

Let's create an webapp image with the followig `Make` commands:


1. To reset Docker images:

   ```
   make clean
   ```

1. Build the image, type:

   ```
   make build
   ```

1. Run the container, type:

   ```
   make run
   ```

4. To stop the docker container, type:

   ```
   make stop
   ```

5. View the application in a browser at `XX.XXX.XXX.XXX:PORT`

	where `XX.XXX.XXX.XXX` is the IP we used in the login step above and `PORT` is the port number provided in the `.env` file earlier.

6. Next, let's deploy your `IBM watsonx chatbot` with the instructions from last [lecture](https://developer.ibm.com/learningpaths/get-started-watson-assistant/configure-and-deploy/):
7. But first edit your Makefile and use a new port: `8001, 8002, 8003, 8004 or 8005`:
   ```
   nano Makefile
   ```

   Edit `PORT` number (8001-8005)

   ```
   export PORT ?= 8001
   ```

   Save file:
   `CTL O`

   Exit file
   `CTL X`

8. Clean, build and run new image with:
   ```
   make clean
   ```
   ```
   make build
   ```
   ```
   make run
   ```
   
## License

This sample code is licensed under the [MIT License](https://opensource.org/licenses/MIT).

## Open Source @ IBM

Find more open source projects on the [IBM Github Page](http://ibm.github.io/)

