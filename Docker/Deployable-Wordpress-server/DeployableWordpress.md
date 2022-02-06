# Projects
## Local deploayable Wordpress website with a MySQL database.

**1. Clone the git file**
```
git clone git@github.com:pdops/Projects.git
```
**2. Install the dependencies (for linux)**
```
sudo apt install docker.io
```
**3 Creating the Wordpress file as well as the database to accompany it**

Now that you have everything necessary , create an empty folder and inside of it create a new file and name it finally adding .yml as a extension opening it with the edditor of your choice. A version needs to be provided to the .yml file so that docker can use the subset of commands that it will understand.

The version used here is 3.7 as it provides all of the necessary commands to run the file successfully:
```
version '3.7'
```
Now that the version has been defined , the services needed for the file are wordpress and a database which will be **MySQL**. Once the services have been named 
you will have to provide them with all of the attributes needed . For the database , the configurations that were added were the image , container_name, volumes, ports, restart and environment. 
1. image - provides the instructions for installing **MySQL** and **WordPress** from [dockerhub](https://hub.docker.com/).
2. container_name - sets the name for the container
3. volumes - creating a location where the files from the database and wordpress will be saved after every restart.
4. ports - exposing a port so that if needed , the host can connect to the database outside of the container.
5. restart - if there is any kind of a error , the container will restart itself.
6. environment - choosing variables and customizing them so that we can access our database.

The environment variables were found on [dockerhubs MySQL page](https://hub.docker.com/_/mysql)
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
As for wordpress service , in order for it to work the **MySQL** container needs to be running before the container starts, and for that to happen the **depends_on** option is used and the database is specified. After that the wordpress image is being specified.  **Volumes** ,**ports** and **restart** have the same functions as the ones for the database , but for the **environemnt** ,wordpress and the appache server need to know how to communicate with the database so the **WORDPRESS_DB_HOST** has been provided with the database service name and port:
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
When both the service and the database are ready the file can be tested by writing **docker-compose up -d** which will build and run the container in the background. To check the containers and the cpu and memory usage use the **docker stats** command.As for testing to see if it works , the best way to do that would be to open a browser and write **"localhost:8888"** as that is the port to which the wordpress website is connected and you should be able to register , create a website  and even after restarting the docker container the data will be present. To shut down the containers you can write **docker-compose down**.
```
docker-compose up -d
docker stats
docker-compose down 
```
This was the entire guide for this Project , hopefully it was helpful.
