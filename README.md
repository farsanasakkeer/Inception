# Inception

## **Containers**
- **Containers** are like lightweight, portable boxes that can run an application along with everything it needs (like code, libraries, and settings).
- They share the operating system of the host machine but are isolated from each other and the host.
- They start up quickly and use fewer resources because they don’t need their own operating system.

### **Containers vs. Virtual Machines**

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



- **Components of Docker**:
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
