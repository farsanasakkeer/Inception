# Inception

## **Containers**
- **Containers** are like lightweight, portable boxes that can run an application along with everything it needs (like code, libraries, and settings).Docker provides a standardized environment for running applications. It packages applications and their dependencies into containers, which are isolated environments. This ensures consistency and reproducibility across different systems.
- They share the operating system of the host machine but are isolated from each other and the host.
- They start up quickly and use fewer resources because they don’t need their own operating system.




https://www.youtube.com/watch?v=Tyy1BUEmhwg&t=217s
The video then delved into the technical aspects of Docker. It explained how to create a Dockerfile, which is a set of instructions for building a Docker image. An image is a read-only template that contains the application code, libraries, and dependencies. The Dockerfile specifies the base image, copies necessary files, installs dependencies, and sets the command to run when the container starts.

Once the Dockerfile is created, the docker build command is used to build the image. The resulting image can then be used to create multiple containers, each representing an isolated instance of the application.

The video also demonstrated how to manage multiple Docker containers using Docker Compose. Docker Compose allows you to define and run multi-container Docker applications using a YAML file. This simplifies the process of setting up and managing complex applications with multiple interconnected services.

https://www.sitepoint.com/what-is-docker/

https://www.techtarget.com/searchitoperations/definition/Docker-image#:~:text=A%20Docker%20container%20can%20use,a%20specific%20point%20in%20time.

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

```
#!/usr/bin/env bash

mkdir $CERT_DIR
openssl req -x509 -newkey rsa:2048 -days 365 -nodes -keyout $CERT_KEY -out $CERT -subj /CN=$HOST_NAME

echo "server {
        listen 443 ssl http2;
        server_name $HOST_LOGIN;

        root   $WP_ROUTE;
        index index.php index.html index.htm index.nginx-debian.html;

        ssl_protocols TLSv1.2;
        ssl_certificate  $CERT;
        ssl_certificate_key  $CERT_KEY;" > $NGINX_CONF;
echo '
        location / {
                try_files $uri $uri/ /index.php?$args =404;
        }

        location ~ \.php$ {
                include snippets/fastcgi-php.conf;
                fastcgi_pass wordpress:9000;
        }
}' >> $NGINX_CONF;

nginx -g "daemon off;"
```
Here's a simple breakdown of what each part of this script does:

**`mkdir $CERT_DIR`**  
   Creates a directory specified by the environment variable `$CERT_DIR` to store your SSL certificate and key.

**`openssl req -x509 -newkey rsa:2048 -days 365 -nodes -keyout $CERT_KEY -out $CERT -subj /CN=$HOST_NAME`**  
   Generates a new self-signed SSL certificate and key using OpenSSL. 
   - **`-x509`**: Creates a self-signed certificate.
   - **`-newkey rsa:2048`**: Generates a new RSA key of 2048 bits.
   - **`-days 365`**: Sets the certificate validity to 365 days.
   - **`-nodes`**: Creates a key without a passphrase.
   - **`-keyout $CERT_KEY`**: Specifies the file to save the private key.
   - **`-out $CERT`**: Specifies the file to save the certificate.
   - **`-subj /CN=$HOST_NAME`**: Sets the Common Name (CN) for the certificate.

**`echo "server { ... }" > $NGINX_CONF`**  
   Writes the beginning part of the Nginx configuration to a file specified by `$NGINX_CONF`.
   - Configures Nginx to listen on port 443 for HTTPS connections.
   - Sets the server name and document root.
   - Configures SSL protocols and specifies the certificate and key files.

**`echo ' ... ' >> $NGINX_CONF`**  
   Appends the remaining configuration to the Nginx configuration file.
   - Configures how Nginx should handle requests:
     - **`location / { ... }`**: Serves files or forwards requests to `index.php` if not found.
     - **`location ~ \.php$ { ... }`**: Configures PHP file handling by forwarding requests to a PHP processor (assumed to be a WordPress service running on port 9000).

**`nginx -g "daemon off;"`**  
   Starts the Nginx web server in the foreground (i.e., not as a background daemon). This is useful in Docker containers to keep Nginx running and manageable.

```
FROM debian:bullseye

RUN apt -y update && apt install -y nginx && apt upgrade -y openssl

COPY ./requirements/nginx/tools/conf.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/conf.sh

ENTRYPOINT ["conf.sh"]
```
Here’s a simple explanation of each part of the Dockerfile:

1. **`FROM debian:bullseye` : Use Debian Bullseye as the base image.**  
   Specifies the base image for your Docker image. In this case, it's Debian Bullseye, which is a version of Debian GNU/Linux.

2. **`RUN apt -y update && apt install -y nginx && apt upgrade -y openssl` : Update the package list, install Nginx, and upgrade OpenSSL.**  
   Updates the package lists, installs the Nginx web server, and upgrades OpenSSL to ensure you have the latest version. The `-y` flag automatically confirms the installation and upgrade.

3. **`COPY ./requirements/nginx/tools/conf.sh /usr/local/bin/` : Copy and make executable a custom script (`conf.sh`)**  
   Copies the `conf.sh` script from your local `./requirements/nginx/tools/` directory on your host machine into the `/usr/local/bin/` directory inside the Docker image.

4. **`RUN chmod +x /usr/local/bin/conf.sh`**  
   Changes the permissions of `conf.sh` to make it executable. This is necessary because the script needs to be run as a command when the container starts.

5. **`ENTRYPOINT ["conf.sh"]`**  
   Sets `conf.sh` as the default command to run when the container starts. This script will be executed when the container is launched.





## mariaDB
```
 #!/usr/bin/env bash

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

wait $mysql_pid #This waits for the MySQL server process, identified by mysql_pid, to complete before proceeding
```
The basic idea of this script is to set up and configure a MySQL server. 

1. **Configure MySQL**: It modifies the MySQL configuration file to allow MySQL to accept connections from any IP address (`bind-address=0.0.0.0`).

2. **Initialize Database**: It initializes the MySQL data directory with necessary database files.

3. **Start MySQL Server**: It starts the MySQL server in the background.

4. **Wait for MySQL Server**: It repeatedly checks to ensure that the MySQL server is up and running by pinging it until a successful response is received.

5. **Set Up Database and User**: Once the server is running, it creates a new database, sets or updates the root user’s password, and creates a new user with specific permissions on the new database.

6. **Ensure Server is Ready**: It waits for the MySQL server process to finish starting up before concluding the script.

Overall, the script automates the process of configuring, starting, and setting up a MySQL server with a specific database and user configuration.

## wordpress 
```
#!/usr/bin/env bash

cd $WP_ROUTE

wp core download --force --allow-root

wp config create --path=$WP_ROUTE --allow-root --dbname=$DB_NAME --dbuser=$DB_USER --dbpass=$DB_PASS --dbhost=$DB_HOST --dbprefix=wp_

if ! wp core is-installed --allow-root --path=$WP_ROUTE; then
wp core install --url=$WP_URL --title=$WP_TITLE --admin_user=$WP_ADMIN_USER --admin_password=$WP_ADMIN_PASS --admin_email=$WP_ADMIN_EMAIL --allow-root
wp user create $WP_USER $WP_EMAIL --role=author --user_pass=$WP_PASS --allow-root
fi

php-fpm7.4 -F
```

This script sets up a WordPress installation inside a specified directory, configures it, installs WordPress if it’s not already installed, creates a new user, and starts the PHP-FPM service to handle PHP requests.

1. **`#!/usr/bin/env bash`**  
   This is the shebang line, which tells the system to use the `bash` shell to interpret the script.

2. **`cd $WP_ROUTE`**  
   Changes the working directory to the path specified by the environment variable `$WP_ROUTE`. This is where WordPress will be installed.

3. **`wp core download --force --allow-root`**  
   Uses the WordPress Command Line Interface (`wp-cli`) to download the latest version of WordPress. The `--force` flag ensures that any existing WordPress files are overwritten, and `--allow-root` allows the command to be run as the root user.

4. **`wp config create --path=$WP_ROUTE --allow-root --dbname=$DB_NAME --dbuser=$DB_USER --dbpass=$DB_PASS --dbhost=$DB_HOST --dbprefix=wp_`**  
   Creates a WordPress configuration file (`wp-config.php`) with the provided database details:
   - `--dbname=$DB_NAME`: Sets the database name.
   - `--dbuser=$DB_USER`: Sets the database user.
   - `--dbpass=$DB_PASS`: Sets the database password.
   - `--dbhost=$DB_HOST`: Sets the database host.
   - `--dbprefix=wp_`: Sets the table prefix to `wp_`.

5. **`if ! wp core is-installed --allow-root --path=$WP_ROUTE; then`**  
   Checks if WordPress has already been installed. If not, the following commands will run.

6. **`wp core install --url=$WP_URL --title=$WP_TITLE --admin_user=$WP_ADMIN_USER --admin_password=$WP_ADMIN_PASS --admin_email=$WP_ADMIN_EMAIL --allow-root`**  
   Installs WordPress with the provided details:
   - `--url=$WP_URL`: Sets the URL of the WordPress site.
   - `--title=$WP_TITLE`: Sets the site title.
   - `--admin_user=$WP_ADMIN_USER`: Sets the admin username.
   - `--admin_password=$WP_ADMIN_PASS`: Sets the admin password.
   - `--admin_email=$WP_ADMIN_EMAIL`: Sets the admin email.

7. **`wp user create $WP_USER $WP_EMAIL --role=author --user_pass=$WP_PASS --allow-root`**  
   Creates a new WordPress user with the role of "author":
   - `$WP_USER`: The username for the new user.
   - `$WP_EMAIL`: The email address for the new user.
   - `--role=author`: Sets the user role to "author".
   - `--user_pass=$WP_PASS`: Sets the user password.

8. **`fi`**  
   Ends the `if` block.

9. **`php-fpm7.4 -F`**  
   Starts PHP-FPM (FastCGI Process Manager) in the foreground. This is necessary for serving PHP files.

```
FROM debian:bullseye

USER root

RUN apt update && apt install -y \
	wget \
	curl \
	sendmail \
	php-mysql \
	php-mysqli \
	mariadb-client \
	php \
	php-fpm && \
	mkdir -p /run/php

RUN curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar && \
	chmod +x wp-cli.phar && \
	mv wp-cli.phar /usr/local/bin/wp

COPY ./requirements/wordpress/conf/www.conf /etc/php/7.4/fpm/pool.d/

COPY ./requirements/wordpress/tools/docker_entry.sh /usr/local/bin/

ENTRYPOINT ["docker_entry.sh"]
```

1. **`FROM debian:bullseye`**  
   Specifies that the Docker image is based on the Debian Bullseye image.

2. **`USER root`**  
   Sets the user for running the following commands to `root`.

3. **`RUN apt update && apt install -y \ ...` : Install necessary tools and software, including PHP, MariaDB client, and `wp-cli`**  
   Updates the package list and installs several packages:
   - **`wget`**: A tool to download files from the web.
   - **`curl`**: A tool to transfer data from or to a server.
   - **`sendmail`**: A mail transfer agent.
   - **`php-mysql`**: PHP extension for MySQL.
   - **`php-mysqli`**: PHP extension for MySQL improved.
   - **`mariadb-client`**: Client tools for MariaDB.
   - **`php`**: The PHP scripting language.
   - **`php-fpm`**: PHP FastCGI Process Manager for handling PHP requests.
   - **`mkdir -p /run/php`**: Creates the directory `/run/php` if it doesn’t exist. This is used by PHP-FPM to store its runtime files.

4. **`RUN curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar && \`**  
   **`chmod +x wp-cli.phar && \`**  
   **`mv wp-cli.phar /usr/local/bin/wp`**  
   Downloads the `wp-cli.phar` file (the WordPress Command Line Interface), makes it executable, and moves it to `/usr/local/bin/` to make it accessible as `wp` from the command line.

5. **`COPY ./requirements/wordpress/conf/www.conf /etc/php/7.4/fpm/pool.d/`**  
   Copies the custom PHP-FPM pool configuration file (`www.conf`) into the Docker image. This file configures PHP-FPM settings.

6. **`COPY ./requirements/wordpress/tools/docker_entry.sh /usr/local/bin/`**  
   Copies the `docker_entry.sh` script into the Docker image, placing it in `/usr/local/bin/`.

7. **`ENTRYPOINT ["docker_entry.sh"]`**  
   Sets `docker_entry.sh` as the default command to run when the container starts.

## Docker compose 

This Docker Compose setup builds and runs three containers (nginx, wordpress, mariadb) connected to a common network and using specific volumes for data persistence. Nginx serves the WordPress site, WordPress runs the application, and MariaDB provides the database backend. The configuration ensures that containers are automatically restarted if they fail and sets up appropriate networking and volume bindings.

**Services**: Define and manage containers, specifying their configuration and behavior.

**Volumes**: Persist and manage data used by containers.

**Networks**: Facilitate communication between containers, allowing them to interact with each other based on network configuration.

```
version: "3.9"
services:
  nginx:
    build:
      dockerfile: ./requirements/nginx/Dockerfile
    container_name: nginx
    env_file:
      - ./.env
    volumes:
      - wordpress:/var/www/html/wordpress
    ports:
      - "443:443"
    networks:
      - web
    depends_on:
      - wordpress
    restart: always
  wordpress:
    build:
      dockerfile: ./requirements/wordpress/Dockerfile
    container_name: wordpress
    env_file:
      - ./.env
    volumes:
      - wordpress:/var/www/html/wordpress
    networks:
      - web
    expose:
      - "9000:9000"
    depends_on:
      - mariadb
    restart: always
  mariadb:
    build:
      dockerfile: ./requirements/mariadb/Dockerfile
    container_name: mariadb
    volumes:
      - mariadb:/var/lib/mysql
    networks:
      - web
    env_file:
      - ./.env
    expose:
      - "3306:3306"
    restart: always
networks:
  web:
volumes:
  wordpress:
    name: wordpress
    driver: local
    driver_opts:
      type: 'none'
      o: 'bind'
      device: '/Users/${USER}/data/wordpress'
  mariadb:
    name: mariadb
    driver: local
    driver_opts:
      type: 'none'
      o: 'bind'
      device: '/Users/${USER}/data/mariadb'
```


The file defines a multi-container Docker application using Docker Compose. It sets up three services: **`nginx`, `wordpress`, and `mariadb`**, along with configurations for networks and volumes.

### Services
---------

1. **nginx**:
   - **`build`**: Uses the Dockerfile located at `./requirements/nginx/Dockerfile` to build the Nginx image.
   - **`container_name`**: Names the container `nginx`.
   - **`env_file`**: Loads environment variables from a `.env` file.
   - **`volumes`**: Mounts the `wordpress` volume to `/var/www/html/wordpress` inside the container, allowing the Nginx server to serve WordPress files.
   - **`ports`**: Maps port 443 on the host to port 443 in the container, which is used for HTTPS.
   - **`networks`**: Connects to the `web` network.
   - **`depends_on`**: Ensures that the `wordpress` container starts before `nginx`.
   - **`restart`**: Automatically restarts the container if it stops.

2. **wordpress**:
   - **`build`**: Uses the Dockerfile located at `./requirements/wordpress/Dockerfile` to build the WordPress image.
   - **`container_name`**: Names the container `wordpress`.
   - **`env_file`**: Loads environment variables from a `.env` file.
   - **`volumes`**: Mounts the `wordpress` volume to `/var/www/html/wordpress` inside the container.
   - **`networks`**: Connects to the `web` network.
   - **`expose`**: Exposes port 9000 for communication within the Docker network, used by PHP-FPM to serve PHP files.
   - **`depends_on`**: Ensures that the `mariadb` container starts before `wordpress`.
   - **`restart`**: Automatically restarts the container if it stops.

3. **mariadb**:
   - **`build`**: Uses the Dockerfile located at `./requirements/mariadb/Dockerfile` to build the MariaDB image.
   - **`container_name`**: Names the container `mariadb`.
   - **`volumes`**: Mounts the `mariadb` volume to `/var/lib/mysql` inside the container, which is where MariaDB stores its data.
   - **`networks`**: Connects to the `web` network.
   - **`env_file`**: Loads environment variables from a `.env` file.
   - **`expose`**: Exposes port 3306 for communication within the Docker network, used by the WordPress container to connect to MariaDB.
   - **`restart`**: Automatically restarts the container if it stops.

### Networks
--------
- **`web`**: Defines a network named `web` that connects all three services (`nginx`, `wordpress`, and `mariadb`).

### Volumes
---------
1. **`wordpress`**:
   - **`name`**: Names the volume `wordpress`.
   - **`driver`**: Uses the `local` driver.
   - **`driver_opts`**: Specifies options for the volume:
     - **`type: 'none'`**: Mounts the directory as a bind mount.
     - **`o: 'bind'`**: Binds to a specific path on the host.
     - **`device: '/Users/${USER}/data/wordpress'`**: Specifies the path on the host for the volume. `${USER}` is replaced with the current username.

2. **`mariadb`**:
   - **`name`**: Names the volume `mariadb`.
   - **`driver`**: Uses the `local` driver.
   - **`driver_opts`**: Specifies options for the volume:
     - **`type: 'none'`**: Mounts the directory as a bind mount.
     - **`o: 'bind'`**: Binds to a specific path on the host.
