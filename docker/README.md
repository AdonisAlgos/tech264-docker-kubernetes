# Learning Docker

![alt text](image.png)

1. Navigate to https://docs.docker.com/desktop/install/windows-install/
2. Click Docker Desktop for Windows - x86_64
3. Follow the installation window.
4. Agree to the Docker service agreenment<br><br>
![alt text](image-1.png)
5. Use the recomended settings<br><br>
![alt text](image-2.png)
6. Confirm and sign in (Create an account if you dont have one)
7. Confirm Docker version by adding the following to a bash shell:

```bash
docker --version
```
8. On Docker Desktop the status should be green and the newer versions also state "Engine running".<br><br>
![alt text](image-3.png)
*Note: If the status is not green, you will likely need to choose to "Run as Administrator". If you don't, you will get an error.*

## Differences between virtualisation and containerisation

* Virtualization creates multiple virtual machines (VMs) on a host system, each with its own guest operating system—including kernel and user space—running on virtualized hardware.
* Containerization isolates applications within containers that share the host system's operating system kernel. Containers package the application code along with its dependencies but do not include a separate OS kernel.

### What is usually included in a Virtualization vs. Containerization
* **Virtualization**: Uses hypervisors to run full OS instances on top of a host OS, each with its own guest OS, consuming more resources.
* **Containerization**: Runs applications in isolated environments using shared OS kernel, leading to smaller, faster, and more efficient resource usage.

### Contents of a Container vs. Virtual Machine
* **Container**: Includes application code, libraries, and dependencies, sharing the host OS kernel.
* **Virtual Machine (VM)**: Includes entire OS, application, libraries, and dependencies, isolated via a hypervisor.

## Benefits of Virtualization:

* Improved Resource Utilization: Virtual machines (VMs) enable multiple operating systems and applications to run on a single physical server, maximizing hardware usage and reducing costs.
* Isolation and Security: Each VM operates in a sandboxed environment, enhancing security by isolating applications from one another.
* Flexibility and Scalability: VMs can be easily created, cloned, or migrated, allowing for rapid scaling and efficient resource management.
* Simplified Management: Virtualization simplifies backup, recovery, and deployment processes since VMs are encapsulated files that can be managed centrally.
* Testing and Development: Provides safe environments for testing new software or updates without impacting production systems.

## Benefits of Containerization:

* Lightweight and Efficient: Containers share the host OS kernel, making them more lightweight and faster to start than VMs.
* Portability: Containers package applications with their dependencies, ensuring consistent behavior across different environments.
* Rapid Deployment: Streamlines the deployment process, enabling continuous integration and delivery pipelines.
* Scalability: Allows for quick scaling of applications to meet demand by spinning up additional container instances.
* Resource Efficiency: Consumes fewer system resources compared to VMs, leading to cost savings.

## Benefits of Virtual Machines over Traditional Architecture:

* Enhanced Resource Optimization: Traditional architectures often underutilize hardware, dedicating entire servers to single tasks. VMs consolidate workloads, maximizing hardware utilization.
* Cost Reduction: Fewer physical servers mean lower hardware, energy, and maintenance costs.
* Improved Disaster Recovery: VMs can be easily backed up, replicated, and restored, enhancing business continuity strategies.
* Hardware Independence: Virtualization abstracts the hardware layer, allowing VMs to run on any physical machine, reducing compatibility issues.
* Faster Provisioning and Deployment: Setting up a new VM is quicker than procuring and configuring a new physical server, accelerating project timelines.

## Microservices

### What are they?

**Microservices** are an architectural approach to software development where an application is structured as a collection of small, independent services. Each service focuses on a specific business capability, runs in its own process, and communicates with other services through lightweight protocols, often HTTP/REST or messaging queues.

### How are they made possible?

* Advancements in Containerization: Tools like Docker enable packaging services with their dependencies, ensuring consistency across environments.
* Orchestration Platforms: Technologies like Kubernetes manage, scale, and orchestrate containers, making it easier to deploy microservices at scale.
* DevOps and CI/CD Pipelines: Automated testing and deployment processes allow for rapid and reliable delivery of microservices.
* APIs and Messaging Systems: Robust communication mechanisms facilitate interaction between services.
* Cloud Computing: Elastic infrastructure provided by cloud platforms supports the dynamic scaling needs of microservices.

### Benefits:

* Scalability: Services can be scaled independently based on demand, optimizing resource usage.
* Flexibility in Technology Stack: Teams can choose the most suitable technology for each service without affecting the entire application.
* Independent Deployment: Services can be updated or deployed without downtime for the whole system.
* Fault Isolation: Issues in one service are less likely to impact others, enhancing system resilience.
* Improved Development Velocity: Smaller, focused teams can work concurrently on different services, accelerating development.
* Easier Maintenance: Simplified codebases for each service make understanding and modifying code more manageable.
* Organizational Alignment: Mirrors modern agile and DevOps practices, promoting collaboration and ownership within teams.

## Docker

### What is Docker?

Docker is an open-source platform that automates the deployment, scaling, and management of applications using containerization. It allows developers to package applications and their dependencies into standardized units called containers, ensuring consistency across different environments.

### Alternatives to Docker:

* Podman: A daemonless container engine for developing, managing, and running containers on Linux systems.
* Containerd: An industry-standard container runtime that manages the complete container lifecycle.
* CRI-O: A lightweight container runtime designed for Kubernetes.
* LXC/LXD: Linux Containers that offer system-level virtualization for running multiple isolated Linux systems.
* rkt (Rocket): A security-focused container engine by CoreOS (note: development has been discontinued).

How Docker Works (Docker Architecture/API):

Docker Architecture:

* Docker Client: The command-line interface or GUI through which users interact with Docker.
* Docker Daemon (dockerd): A background service that handles building, running, and distributing containers.
* Docker Registries: Repositories like Docker Hub where Docker images are stored and shared.

Docker Objects:

* Images: Read-only templates used to create containers.
* Containers: Runnable instances of images that include the application and its environment.
* Networks and Volumes: Facilitate communication between containers and manage persistent data storage.

How It Works:

* Users issue commands via the Docker Client, which communicates with the Docker Daemon through a REST API over UNIX sockets or network interfaces.
* The Docker Daemon performs tasks such as building images, running containers, and managing networks and storage.
* Docker uses a layered filesystem and union mounts to efficiently build and distribute container images.

Success Story Using Docker:

**Spotify**:

* Spotify adopted Docker to enhance its development and deployment processes.
* By containerizing services, they achieved consistent environments from development to production.
* Docker enabled rapid scaling of microservices, improved resource utilization, and faster deployment times.
* The transition to Docker contributed to increased developer productivity and operational efficiency.