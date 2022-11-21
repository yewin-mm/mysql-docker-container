<!-- PROJECT SHIELDS -->

<!--
*** I'm using markdown "reference style" links for readability.
*** Reference links are enclosed in brackets [ ] instead of parentheses ( ).
*** See the bottom of this document for the declaration of the reference variables
*** for contributors-url, forks-url, etc. This is an optional, concise syntax you may use.
*** https://www.markdownguide.org/basic-syntax/#reference-style-links
-->
[![Contributors][contributors-shield]][contributors-url]
[![Forks][forks-shield]][forks-url]
[![Stargazers][stars-shield]][stars-url]
[![Issues][issues-shield]][issues-url]
[![LinkedIn][linkedin-shield]][linkedin-url]

<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
[contributors-shield]: https://img.shields.io/github/contributors/yewin-mm/mysql-docker-container.svg?style=for-the-badge
[contributors-url]: https://github.com/yewin-mm/mysql-docker-container/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/yewin-mm/mysql-docker-container.svg?style=for-the-badge
[forks-url]: https://github.com/yewin-mm/mysql-docker-container/network/members
[stars-shield]: https://img.shields.io/github/stars/yewin-mm/mysql-docker-container.svg?style=for-the-badge
[stars-url]: https://github.com/yewin-mm/mysql-docker-container/stargazers
[issues-shield]: https://img.shields.io/github/issues/yewin-mm/mysql-docker-container.svg?style=for-the-badge
[issues-url]: https://github.com/yewin-mm/mysql-docker-container/issues
[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?style=for-the-badge&logo=linkedin&colorB=555
[linkedin-url]: https://www.linkedin.com/in/ye-win-1a33a292/


# mysql-docker-container
* This is to run MySQL as docker container. 
* You can containerized MySQL db as my step by step guide as below.

<!-- TABLE OF CONTENTS -->
## Table of Contents
- [About The Project](#about-the-project)
	- [Built With](#built-with)
- [Getting Started](#getting-started)
	- [Before you begin](#before-you-begin)
	- [Clone Project](#clone-project)
	- [Prerequisites](#prerequisites)
	- [Instruction](#instruction)
      - [1. Create Network](#create-network)
      - [2. Create Volume](#create-volume)
      - [3. Run MySQL container](#run-container)
      - [4. Check MySQL Container](#check-container)
      - [5. Check Network driver and Volume](#check-network-volume)
      - [6. Connect DB Container From Other Containers](#connect-from-other-container)
      - [7. Conclusion](#conclusion)
- [Contact Me](#contact)
- [Contributing](#Contributing)


<a name="about-the-project"></a>
## ⚡️About The Project
This is to run MySQL database as container. Containerization is the good approach to run any application in container. <br>
You can start, stop this container as you want. <br>
If you want to do things which related with database, you can start container. <br>
If you don't want to do things with db, you can stop container. <br>
If you install MySQL db with installer, your db will run in background the while time, <br> 
and it will impact your machine RAM as running background even if you don't want to use it. <br>
So, Containerize MySQL db is the better approach than manual installation. <br>
You can do containerize other db like PostgreSQL, etc as you want. All you need is your want db image is existed in docker hub. <br>
You can do step by step as I showed in [Instruction](#instruction) section. <br>
Example project to connect this Containerized MySQL DB can be found here, [spring-boot-jpa-docker](https://github.com/yewin-mm/spring-boot-jpa-docker).

<a name="built-with"></a>
### 🪓 Built With
This project is built with
* [Docker](https://www.docker.com/products/docker-desktop/)


<a name="getting-started"></a>
## 🔥 Getting Started
This project purpose to run MySQL db as container. <br>
So, you need to install `Docker` in your machine. [Get Docker](https://www.docker.com/products/docker-desktop/) <br>
So, please make sure all are installed in your machine. <br>
You can do step by step as I showed in [Instruction](#instruction) section.


<a name="before-you-begin"></a>
### 🔔 Before you begin
Before learn and see this project, <br>
You should already know some docker command and you can learn some commands in the instructions section of this [spring-boot-docker-sample project](https://github.com/yewin-mm/spring-boot-docker-sample). <br>
Example project to connect this Containerized MySQL DB can be found here, [spring-boot-jpa-docker](https://github.com/yewin-mm/spring-boot-jpa-docker).


<a name="clone-project"></a>
### 🥡 Clone Project
* Clone the repo
   ```sh
   git clone https://github.com/yewin-mm/mysql-docker-container.git


<a name="prerequisites"></a>
### 🔑 Prerequisites
Prerequisites can be found in here [Spring Boot Application Instruction](https://github.com/yewin-mm/spring-boot-app-instruction).


<a name="instruction"></a>
### 📝 Instruction

<a name="create-network"></a>
#### 1. Create network

* One of the reasons for Docker Network is Docker containers and services are so powerful is that you can connect them together, or connect them to non-Docker workloads. (ref - docker official side)
* Whatever, you don't use custom network or use network is ok. But here, I will use my custom network.
  *There are 5 network drivers, Bridge, Host, Overlay, MacVLAN, None.
* `Bridge` is default network and all containers linked this network if not specify exactly and it have an internal IP address which they (containers) communicate with each other easily.
* `Host` is public network which use host ip address and multiple containers can't run on this host network. You can go search for more about network types in google.
* Here, I will go my MySQL db with my custom ***Bridge*** network name called `mysql` which will go under `Bridge` network driver.
* Open `Docker Desktop`.
* Type in your cmd or terminal below commands.

```sh
docker network create mysql
````
* here, you can give network name as you want.
* here, you can check your network by typing `docker network ls`
* here, you can delete docker network by typing `docker network rm {your_network_name}`, to know the active network, you can check by typing above `docker network ls` command.
* If you want to delete docker network, you have to disconnect or delete container first which connect to this network.

<a name="create-volume"></a>
#### 2. Create volume

* For some case, what if we delete the database container and if we start the container from image again?
* Yes, it will delete your old database data too.
* For that case, we need volume to backup our data (persist our data)
* eg. sharing our data to other folder which outside of our db container or take data from outside of our db container like store data in other directory.
* There are two ways to backup our data.
	* Volume
	* Bind mount

	* Volumes are easier to back up or migrate than bind mounts and volumes is control by docker as it's created inside docker.
	* Bind mount is a just directory (folder) on the host (laptop) file system which is like storage area which doesn't depend (own) on the container.
	* Here, I will use Volume for persisting our data.

  ```sh
  docker volume create mysql-volume
  ```

	* You can check your volume by typing `docker volume ls`.
	* If you want to delete volume, you can type `docker volume rm {your_volume_name}`, to know the active volume, you can check by typing above `docker volume ls` command.


<a name="run-container"></a>
#### 3. Run MySQL container
* Normally, if you want to run container, you need to have docker images first.
* You can get images by pulling images by `docker pull {image_name} from docker hub if exist or creating (build) your own images if not exist in docker hub.
* In here, MySQl images is alredy in Docker Hub cloud and if exist in docker hub, you don't need docker pull command. It will pull automatically when docker container run.
* So, you can skip pulling MySQL image from Docker hub because if docker can't find existing local images, it will auto pull from docker hub.
* But if you want to go step by step, you can type `docker pull mysql:latest` and after pulling was finished, you can type below docker run command.
* You can just directly type docker run command rather than `docker pull {image name}` command like below.

```sh
docker run --name=yw_mysql -p3306:3306 --network mysql -v mysql-volume:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root --restart unless-stopped -d mysql:latest
```

* here, --name is your continer name and you can give as you want.
* -p mapping container port
* --network is to run our app under our custom define network which is in first step.
* -v or --volume is to connect our created volume which is in above step and give directory of the volumn which volume directory is inside docker as I mentioned in above docker will control that volume. Path is inside container and you can give as your want.
* -e option is ENVIRONMENT variable which is to define, root password, user, password, database, etc. (if you create user with MYSQL_USER environment, you need to give permission)
* --restart need to add policy, there are 4 policy, `no` (default, if no set), `on-failure`, `always`, `unless-stopped`.
	* I used `unless-stopped` policy because I don't want auto restart my MySQL container on both manually stopped condition and docker deamon stopped conditions. You can use `always` policy if you want auto restart after docker application was started. for more details about restart policy, you can check in [Docker restart policy](https://docs.docker.com/config/containers/start-containers-automatically/).
* -d is for detach mode (run in background and hiding logs), you can look logs for your container by typing `docker logs -f -n 100 {container_id}`
* latest is for MySQL latest version and you can add other version as you want like mysql:5.7, etc. (current is 8.0.31)

<a name="check-container"></a>
#### 4. Check MySQL Container
* Check MySQL images is already in or not by typing

```sh 
docker images
```
* Here, you can see image name, image id, etc.

* Check MySQL container is running well or not by typing

```sh 
docker ps
```
* Here, you can see image name, container name, container id, ports, etc.

* Go inside MySQL docker container

```sh
docker exec -it {mysql_container_id} /bin/bash
```

* here, you can leave container by typing `exit` command inside container.
* you can also get inside container by container name like `docker exec -it {mysql_container_name} /bin/bash`
* So, you can use container name for clear text instead of using long container id for doing the actions with related with container.

* after you get inside container, enter MySQL console
```sh
mysql -uroot -proot
```
* here, -u is for username and -p is for password.
* here, if you get login into MySQL Constol with above, you can go directly to [Successfully Login](#successfully-login) section.
* but, if you can't go MySQL console by authentication root.
* type `exit` to leave container.
<a name="clear"></a>
###### Clear
* Clear everything by below steps.
* Delete Container, before deleting container, you need to stop MySQL container by typing `docker stop {mysql_conatiner_id}`, to know the mysql container id, you can type `docker ps`.
* Delete(remove) MySQL container by typing `docker rm {mysql_conatiner_id}`.
* After container was deleted, you can delete volume by typing `docker volume rm {volume_name}`, to know your volume that you created in above No.1, you can type `docker volume ls` command.
* After container was deleted, you can delete image by typing `docker rmi {mysql_image_id}`, to know the mysql image id, you can type `docker images`.
* You can even delete the network, actually, network is no need to delete if you want clear your existing db cache. Above steps is enough.
* After everything was clear, Run below command with create user (name-user, password-root) while running MySQL container like below.

```sh
docker run --name=yw_mysql -p3306:3306 --network mysql -v mysql-volume:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root -e MYSQL_USER=user -e MYSQL_PASSWORD=root --restart unless-stopped -d mysql:latest
```

* Here, I added user and password while running MySQL container by using environment.
* After running MySQL container with adding environment user and password again, check container and go inside MySQL container.
* After you get inside container, you can test login into mysql console by typing `mysql -uroot -proot` again or type `mysql -uuser -proot`.
* If you get inside Mysql Console with root user it's ok and do below process, but you can only get inside only with user, you need to grant access to this user as that user doesn't has any privileges for now. for more details, find in google.

<a name="successfully-login"></a>
###### Successfully Login into MySQL Console
* After you get inside MySQL console successfully with root user, please type below command to check host is allow for public or not.
* Type in MySQl console (inside MySQL container, inside MySQL console).
```sh
SELECT host, user FROM mysql.user;
```

* here, if you saw like below, it's ok.

| host      | user             |
|-----------|------------------|
| %         | root             |
| localhost | mysql.infoschema |
| localhost | mysql.session    |
| localhost | mysql.sys        |
| localhost | root             |


* Please note that, `%` under `host` tab is bind with root user should be seen. It's mean allow any host with root user.
* If you see that `%` with `host` like above table, 
* That means it's ok and you can go directly to this [check network and volume](#check-network-volume) section by skip below steps.
* But if you don't see `%` under host tab,
	* You need to to allow any host to connect db to your user eg. root user. 
	* Type

  ```sh
  UPDATE mysql.user SET host='%' WHERE user='root';
  ```
	* and type `FLUSH PRIVILEGES;` to reload all changes.
	* and type `SELECT host, user FROM mysql.user;` again and check host is `%` for root user or not.
	* If you see that `%` host with `root` user, you can go directly to this [check network and volume](#check-network-volume) section by skip below steps.
	* But if you still don't see `%` in host or your above update query is not ok, Create other users like below and update to that user for host to `%`.


<a name="create-user"></a>
###### create other user
* You can create other users in MySQL console to connect db by typing
```sh
CREATE USER 'test'@'%' IDENTIFIED BY 'testpassword';
 ```
* Here, sample username is `test` and password is `testpassword` and you can change username and password as you want.
 
* Give grant access permission for creating table, etc to that new user by
```sh
GRANT ALL PRIVILEGES ON *.* TO 'test'@'%';
```
* here, test is username, and you need to use your given username in there.

* For some case with creating new users, if you want to set host to `%` with that users, you can update like below
```sh
UPDATE mysql.user SET host='%' WHERE user='test';
```

* Update changes,
```sh
FLUSH PRIVILEGES;
```

* If you create other users with `localhost` instead of `%` like <br> `CREATE USER 'test'@'localhost' IDENTIFIED BY 'root';` query or and grant permission with `GRANT ALL PRIVILEGES ON *.* TO 'test'@'localhost';`,
	* For that above case, you need to set host to `%` like `UPDATE mysql.user SET host='%' WHERE user='test';` and type `FLUSH PRIVILEGES;` to reload all changes.


* If you ok with root user and host is `%`, you can use that root user from outside like your internal IDE properties file to connect MySQL db or you can connect with GUI by using that user. (db host - localhost, db username - root, db password - root).
* If you ok with other new user for host is `%`, you need to connect by using that new user name and password from outside. (db host - localhost, db username - your given name, db password - your given password).

* If you still error with host to `%`.
	* Type `exit` to leave MySQL console and type `exit` again to leave container and do clear all (container, volume, images) that I already dropped step by step in above [clear](#clear) section.
	* And run docker mysql container with below command and test like above steps.

  ```sh
  docker run --name=yw_mysql -p3306:3306 --network mysql -v mysql-volume:/var/lib/mysql -e MYSQL_ROOT_HOST=% -e MYSQL_ROOT_PASSWORD=root -e MYSQL_USER=user -e MYSQL_PASSWORD=root --restart unless-stopped -d mysql:latest
  ```
	* Here, I added root host to `%`.
	* But you need to do testing above steps for sure like testing host by `SELECT` query.
	* If not still ok with root user, you can test by creating other user like above steps.

* If everything was fine, type `exit` to leave MySQL console and type `exit` again to leave container.


<a name="check-network-volume"></a>
#### 5. Check Network driver and Volume
* If you are in MySQL console or MySQL container, type `exit` to leave MySQL console and type `exit` again to leave container.
* You can check Network driver and Volume as below command
```sh
docker inspect {mysql_container_id}
```

* here, you may see a lot of setting for your container,
* scroll down and you can see `Networks` tab under `NetworkSettings` in there and check `mysql` is under `Networks` that we already create network in first step.
* Which mean our container is going under our custom define network.
* scroll up and you can see `Binds": [
  "mysql-volume:/var/lib/mysql"
  ]` under `HostConfig` tab and you can see `mysql-volume` that we already create volume in second step.
* Which mean our container is attach with our custom volume.

* If not see `mysql` network or `mysql-volume`, please do clear all like above [clear](#clear) secion 
* And run mysql container again and please make sure in run command for `--network mysql` and `-v mysql-volume:/var/lib/mysql` are same with your created network name and volume name.


<a name="connect-from-other-container"></a>
#### 6. Connect DB Container From Other Containers
* If everything was fine for running MySQL db as container.
* Add database username to `root` or `your given name` and password to `root` or `your given password` in your application properties file.
* If you don't want to use `root` user, you can create user as per this [create other user](#create-user) section.
* You can connect Containerized MySQL db from outside application (your host, like running in IDE or run as jar) by adding `localhost` or `127.0.0.1` in database datasource url connection string in your application properties file.
* But you can't connect to Containerized MySQL db with that `localhost` or `127.0.0.1` when your application was running as a container.
* There are two ways to call containerized MySQL db from containerized application which to add in your application properties file in your db connection string
	* Using container name
		* You need to add mysql container name to connect between container.
		* Add below db url connection string in your application properties file.
		* eg. `spring.datasource.url=jdbc:mysql://yw_mysql:3306/{your_database_name}`
		* Here, I used `yw_mysql` because I set name for container as `yw_mysql` in docker run command.
	* Using container ipaddress
		* I suggest to use above container name instead of using this below container ipaddress.
		* You can find ipaddress to connect between containers
		* Type in cmd or terminal

	  ```sh
      docker inspect {mysql_container_id} | grep -i ipaddress
      ```
		* Here, you can see address like "IPAddress": "172.20.0.2".
		* Add below db url connection string in your application properties file.
		* eg. `spring.datasource.url=jdbc:mysql://172.20.0.2:3306/{your_database_name}`

* I recommend to use container name instead of using ipaddress while connection to containerized db from containerized application.
* If you don't want to run your application as container, you can use `localhost` or `127.0.0.1` which I mentioned in above.
* Example project to connect Containerized MySQL DB can be found here, [spring-boot-jpa-docker](https://github.com/yewin-mm/spring-boot-jpa-docker).

<a name="conclusion"></a>
#### 7. Conclusion
* Finally, you could run MySQL database as container.
* If you don't want to do things which related with database, 
* You can manage with start or stop your container by `docker stop {mysql_container_id}`, `docker start {mysql_container_id}`.
* You can get your stopped MySQL container id by `docker ps -a` because you can't get container id by `docker ps`, if it is stopped.
* So, If you want to use MySQL, you can `start`. No need to re-run the whole process, it's just `docker start {mysql_container_id}` to get that stopped container id, you need to type `docker ps -a`.
* If you don't want MySQL as for a while, you can `stop`. And if you want back, you can `start` again like above `docker ps -a` and `docker start {mysql container_id}`.
* So, it will reduce RAM space on your laptop to get better performance than you install MySQL database with installer because installer will run application in background the whole time.
* Example project to connect Containerized MySQL DB can be found here, [spring-boot-jpa-docker](https://github.com/yewin-mm/spring-boot-jpa-docker).

***Have Fun and Enjoy in Learning Code***


<a name="contact"></a>
## ✉️ Contact
Name - Ye Win <br> LinkedIn profile -  [Ye Win's LinkedIn](https://www.linkedin.com/in/ye-win-1a33a292/)  <br> Email Address - yewin.mmr@gmail.com

Project Link: [MySQL Docker Container](https://github.com/yewin-mm/mysql-docker-container)



<a name="contributing"></a>
## ⭐ Contributing
Contributions are what make the open source community such an amazing place to be learn, inspire, and create. Any contributions you make are **greatly appreciated**.
<br>If you want to contribute....
1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/yourname`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push -u origin feature/yourname`)
5. Open a Pull Request
