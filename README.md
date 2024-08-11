# Inception

## **Containers**
- **Containers** are like lightweight, portable boxes that can run an application along with everything it needs (like code, libraries, and settings).Docker provides a standardized environment for running applications. It packages applications and their dependencies into containers, which are isolated environments. This ensures consistency and reproducibility across different systems.
- They share the operating system of the host machine but are isolated from each other and the host.
- They start up quickly and use fewer resources because they don’t need their own operating system.

## **Containers vs. Virtual Machines**

1. **Containers**:
   - **Size**: Smaller because they share the host OS.
   - **Speed**: Start and run quickly.
   - **Isolation**: Applications run in their own environments but share the same OS.
   - **Use**: Great for running multiple applications on the same host efficiently.

2. **Virtual Machines (VMs)**:
   - **Size**: Larger because each VM includes its own full operating system.
   - **Speed**: Slower to start because they need to boot up their own OS.
   - **Isolation**: Fully isolated with their own OS, making them heavier.
   - **Use**: Suitable for running different operating systems or when you need complete isolation from the host.


- **Containers** are like lightweight, efficient packages for applications that share the host’s OS.
- **VMs** are like separate computers with their own OS, which are heavier and slower to start. 

Containers are more efficient and faster but offer less isolation compared to VMs.



## **Components of Docker**:
  - **Docker Engine**: The runtime that enables containers to run on a host machine.A Docker image is similar to a snapshot in other types of VM environments. It's a record of a Docker container at a specific point in time
  - **Docker CLI**: Command-line interface to interact with Docker.
  - **Docker Daemon**: Manages Docker containers and images.
  - **Docker Hub**: A cloud-based repository for sharing Docker images.

### **Docker Image and Container**
- **Docker Image**: A lightweight, standalone, and executable package that includes everything needed to run a piece of software (code, runtime, libraries, environment variables, and config files).
- **Docker Container**: A running instance of a Docker image that operates in an isolated environment.


## **NGINX**

**NGINX** is a tool that helps deliver websites to users. 

- **Web Server**: It sends web pages to your browser when you visit a website.
- **Reverse Proxy**: It can forward your requests to other servers.
- **Load Balancer**: It spreads incoming traffic across multiple servers to keep things running smoothly.
- **Cache**: It stores copies of popular pages to speed up loading times.

In short, NGINX helps make websites faster and more reliable.


Here’s a simple example to illustrate how NGINX works as a reverse proxy:

## **Example: Using NGINX as a Reverse Proxy**

Imagine you have a website with two parts:

1. **A main web server** that handles general requests (e.g., homepage, blog posts).
2. **An application server** that handles specific tasks (e.g., user login, data processing).

Without a reverse proxy, users would directly access these servers. However, you want to streamline the process and add a layer of security.

### **How NGINX Works as a Reverse Proxy**

1. **User Request**: A user wants to log in to your website and types the URL into their browser.
   
2. **NGINX as a Gatekeeper**: The request first goes to NGINX, which is set up as a reverse proxy.

3. **Forwarding Requests**:
   - If the request is for the main website content (like the homepage), NGINX sends it to the main web server.
   - If the request is for login or data processing, NGINX forwards it to the application server.

4. **Response**: Once the servers process the request, they send the response back to NGINX.

5. **User Receives the Response**: NGINX then sends the final response back to the user’s browser.

https://www.youtube.com/watch?v=Tyy1BUEmhwg&t=217s
The video then delved into the technical aspects of Docker. It explained how to create a Dockerfile, which is a set of instructions for building a Docker image. An image is a read-only template that contains the application code, libraries, and dependencies. The Dockerfile specifies the base image, copies necessary files, installs dependencies, and sets the command to run when the container starts.

Once the Dockerfile is created, the docker build command is used to build the image. The resulting image can then be used to create multiple containers, each representing an isolated instance of the application.

The video also demonstrated how to manage multiple Docker containers using Docker Compose. Docker Compose allows you to define and run multi-container Docker applications using a YAML file. This simplifies the process of setting up and managing complex applications with multiple interconnected services.

https://www.sitepoint.com/what-is-docker/

https://www.techtarget.com/searchitoperations/definition/Docker-image#:~:text=A%20Docker%20container%20can%20use,a%20specific%20point%20in%20time.


## mariadb
` #!/usr/bin/env bash

echo >> $DB_CONF_ROUTE
echo "[mysqld]" >> $DB_CONF_ROUTE
echo "bind-address=0.0.0.0" >> $DB_CONF_ROUTE #allows MySQL to listen for connections from any IP address, not just localhost.

mysql_install_db --datadir=$DB_INSTALL

mysqld_safe &
mysql_pid=$!

until mysqladmin ping >/dev/null 2>&1; do
  echo -n "."; sleep 0.2
done

mysql -u root -e "CREATE DATABASE $DB_NAME;
    ALTER USER 'root'@'localhost' IDENTIFIED BY '$DB_ROOT_PASS';
    GRANT ALL ON $DB_NAME.* TO '$DB_USER'@'%' IDENTIFIED BY '$DB_PASS';
    FLUSH PRIVILEGES;"

wait $mysql_pid #This waits for the MySQL server process, identified by mysql_pid, to complete before proceeding `
The basic idea of this script is to set up and configure a MySQL server. 

1. **Configure MySQL**: It modifies the MySQL configuration file to allow MySQL to accept connections from any IP address (`bind-address=0.0.0.0`).

2. **Initialize Database**: It initializes the MySQL data directory with necessary database files.

3. **Start MySQL Server**: It starts the MySQL server in the background.

4. **Wait for MySQL Server**: It repeatedly checks to ensure that the MySQL server is up and running by pinging it until a successful response is received.

5. **Set Up Database and User**: Once the server is running, it creates a new database, sets or updates the root user’s password, and creates a new user with specific permissions on the new database.

6. **Ensure Server is Ready**: It waits for the MySQL server process to finish starting up before concluding the script.

Overall, the script automates the process of configuring, starting, and setting up a MySQL server with a specific database and user configuration.

