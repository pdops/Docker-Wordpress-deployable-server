# Projects
## Local deploayable Wordpress website with a mysql database.

**1. Clone the git file**
```
git clone git@github.com:pdops/Projects.git
```
**2. Install the dependencies (for linux)**
```
sudo apt install docker.io
```
**3 Creating the Wordpress file as well as the database to accompany it**

Now that you have everything necessary , create an empty folder and inside of it create a new file and name it finally adding .yml as a extension opening it with the edditor of your choice. A version needs to be provided to the .yml file so that docker can so that docker can use the subset of commands that docker will understand. The version of used here is 3.7 as it provides all of the necessary commands to run the file successfully.

```
version '3.7'
```
Now that the version has been defined , the services needed for the first file are wordpress and a database which will be mysql. Once the services have been named 
you will have to provide them with all of the attributes needed . For the database , the configurations that were added were the image , container_name, volumes, ports, restart and environment. 
1. image - provides the instructions for installing mysql and wordpress from dockerhub.
2. container_name - sets the name for the container
3. volumes - creating a location where the files from the database and wordpress will be saved after every ended session so that the progress that was made is not lost.
4. ports - exposing the mysql ports so that we are able to connect from the host using the phpmyadmin.yml file that was created.
5. restart - if for any reason the container fails it can get restarted and fix the issue.
6. environment - choosing variables and customizing them so that we can access our database.
```
services:
  db:
    image: mysql:8.0
    container_name:wpsampledb
    volumes:
      - /db_data:var/lib/mysql
    ports:
      -"8886:3306"
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: somewordpress
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
```
As for wordpress service , in order for it to work the mysql container needs to be already running before the container even starts, and for to happen the **dpends_on** command is used and the database is specified. After that the image imported is for the wordpress website and it is using the latest version of it. The **container_name** , **volumes** ,**ports** and **restart** have the same functions as the ones for the database , but for the **environemnt** ,wordpress and the appache server need to know how to communicate with the database so the **WORDPRESS_DB_HOST** has been provided with the database service name and port:
```
  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    container_name: wpsample
    volumes:
     - /html:/var/www/html
    ports:
     - "8888:80"
    restart: always
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress 
```
When both the service and the database are ready the file can be tested by writing **docker-compose up -d** which will compose and run the container in the background. To check the containers and the cpu and memmory usage the **docker stats** command will be able to show it. As for testing to see if it works , the best way to do that would be to open a browser and write **"localhost:8888"** as that is the port to which the wordpress website is connected and you should be able to register , create a website and even after shutting down the docker container after putting it back up you will still be at the same place where you finished off. To shut down the containers you can write **docker-compose down**
```
docker-compose up -d
docker stats
docker-compose down 
```
This was the entire guide for this Project , hopefully it was helpful.