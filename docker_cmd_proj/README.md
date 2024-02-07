Test docker project describing steps for creating system of two docker containers based on my images. 
The images create from  raw file system collected by debootstrap.
I do not use Dockerfile at all.
Containers not interacting with each others.

### Create images
```
debootstrap bullseye rootfs2
tar -czf rootfs2.tar.gz -C ~/base-docker-image/docker_cmd_proj rootfs2
```

### first - hello image

```
docker run -it --name=base_cnt base_img:v1 /bin/bash
```
Then in container base_cnt
```
>>> apt install python3
>>> mkdir app && cd app
>>> create main.py file with following code: 
		print("Hello!")

```
Create new image 1 with changes
```
docker commit base_cnt base_img_hello:v2
```


### second - loop image

```
docker run -it --name=base_cnt_2 base_img:v1 /bin/bash
```
Then in container base_cnt_2
```
>>> apt install python3
>>> mkdir app && cd app
>>> create loop.py file with following code:
		import time
		x = 0
		while x <= 30:
			time.sleep(180)
			x += 1
			print(f"sleep {x} times ")
```
Create new image 2 with changes
```
docker commit base_cnt base_img_loop:v2
```


### create archives
```
docker image tag base_img_loop:v2 base_img_loop:latest
docker image tag base_img_hello:v2 base_img_hello:latest
docker save -o base_img_hello.tar base_img_hello:latest
docker save -o base_img_loop.tar base_img_loop:latest
```

### Run and Output
```
root@alisa:~/base-docker-image/docker_cmd_proj# docker-compose up --build
Creating network "docker_cmd_proj_default" with the default driver
Creating docker_cmd_proj_hello_1 ... done
Creating docker_cmd_proj_loop_1  ... done
Attaching to docker_cmd_proj_loop_1, docker_cmd_proj_hello_1
hello_1  | Hello!
docker_cmd_proj_hello_1 exited with code 0

...

root@alisa:~/base-docker-image/docker_cmd_proj# docker-compose up -d
docker_cmd_proj_loop_1 is up-to-date
Starting docker_cmd_proj_hello_1 ... done

root@alisa:~/base-docker-image/docker_cmd_proj# docker ps -a
CONTAINER ID   IMAGE                   COMMAND             CREATED         STATUS                      PORTS     NAMES
96d82eee06fc   base_img_loop:latest    "python3 loop.py"   2 minutes ago   Up 2 minutes                          docker_cmd_proj_loop_1
4565168440a9   base_img_hello:latest   "python3 main.py"   2 minutes ago   Exited (0) 21 seconds ago             docker_cmd_proj_hello_1

```
There are deleted all archives couse of large size and stayed onle instructions
