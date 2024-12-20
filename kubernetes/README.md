# Learning Kubernetes

- [Learning Kubernetes](#learning-kubernetes)
  - [Kubernetes Reasearch](#kubernetes-reasearch)
    - [Why is Kubernetes Needed?](#why-is-kubernetes-needed)
    - [Benefits of Kubernetes](#benefits-of-kubernetes)
    - [Success Stories](#success-stories)
    - [Kubernetes Architecture](#kubernetes-architecture)
    - [What is a Cluster?](#what-is-a-cluster)
    - [Master vs. Worker Nodes](#master-vs-worker-nodes)
    - [Pros and Cons of Using Managed Service vs. Launching Your Own](#pros-and-cons-of-using-managed-service-vs-launching-your-own)
    - [Control Plane vs. Data Plane](#control-plane-vs-data-plane)
    - [Kubernetes Objects](#kubernetes-objects)
    - [What Does it Mean a Pod is "Ephemeral"?](#what-does-it-mean-a-pod-is-ephemeral)
    - [How to Mitigate Security Concerns with Containers](#how-to-mitigate-security-concerns-with-containers)
    - [Maintained Images](#maintained-images)
  - [Using Kubernetes](#using-kubernetes)
    - [Kubernetes Setup](#kubernetes-setup)
    - [Kubernetes Commands](#kubernetes-commands)
    - [Creating Objects: Deployments](#creating-objects-deployments)
    - [Creating Objects: Service](#creating-objects-service)
  - [Task: K8s deployment of NodeJS Sparta test app](#task-k8s-deployment-of-nodejs-sparta-test-app)
    - [Creating the application](#creating-the-application)
      - [Step 1: Create the deployement](#step-1-create-the-deployement)
      - [Step 2: Create the Service](#step-2-create-the-service)
    - [Creating the database](#creating-the-database)
      - [Step 1: Create the deployement](#step-1-create-the-deployement-1)
      - [Step 2: Create the Service](#step-2-create-the-service-1)
    - [Step 3: Seeding the database: Method 1](#step-3-seeding-the-database-method-1)
    - [Step 3: Seeding the database (Manual Approach): Method 2](#step-3-seeding-the-database-manual-approach-method-2)
  - [Task: Create 2-tier deployment with PV for database](#task-create-2-tier-deployment-with-pv-for-database)
    - [Step 1: Create the Persistent Volume (PV)](#step-1-create-the-persistent-volume-pv)
    - [Step 2: Create the Persistent Volume Claim (PVC)](#step-2-create-the-persistent-volume-claim-pvc)
    - [Step 3: Create the MongoDB Deployment with the PVC](#step-3-create-the-mongodb-deployment-with-the-pvc)
  - [Task: Research types of autoscaling with K8s](#task-research-types-of-autoscaling-with-k8s)
  - [Task: Use Horizontal Pod Autoscaler (HPA) to scale the app](#task-use-horizontal-pod-autoscaler-hpa-to-scale-the-app)
    - [Step 1: Creating and configuring the Horizontal Pod Autoscaler (HPA)](#step-1-creating-and-configuring-the-horizontal-pod-autoscaler-hpa)
    - [Step 2: Setup the Metrics server](#step-2-setup-the-metrics-server)
    - [Step 3: Load Test the Application](#step-3-load-test-the-application)
    - [Step 4: Monitor the Kubernetes Cluster.](#step-4-monitor-the-kubernetes-cluster)

## Kubernetes Reasearch

### Why is Kubernetes Needed?

Kubernetes is needed to manage containerized applications in a clustered environment. It automates the deployment, scaling, and operation of application containers across clusters of hosts. This orchestration simplifies the complexities of managing microservices and distributed systems, ensuring applications run reliably and efficiently.

### Benefits of Kubernetes

* **Rollback Changes**
* **Load-Balancing**
* **Open-Source**
* **Self-Healing**
* **Scalability**: Automatically scales applications up or down based on demand.
* **High Availability**: Provides self-healing mechanisms like auto-restarting, rescheduling, and replicating containers.
* **Portability**: Works across various environments—from on-premises to public clouds—ensuring consistent operations.
* **Resource Optimization**: Efficiently utilizes infrastructure resources by managing container placement and resource allocation.
* **Extensibility**: Offers a modular architecture with a rich ecosystem of plugins and integrations.

### Success Stories

* **Spotify**: Adopted Kubernetes to manage its microservices architecture, improving deployment speed and resource utilization.
* **The New York Times**: Migrated to Kubernetes to streamline their content delivery, resulting in faster deployment times and improved scalability.
* **CERN**: Uses Kubernetes to handle vast amounts of data processing for particle physics research, enhancing computational efficiency.
* **Airbnb**: Implemented Kubernetes to better manage scaling demands, leading to more efficient infrastructure management.

### Kubernetes Architecture

Kubernetes architecture is based on a master-worker model designed for orchestrating containerized applications across clusters of hosts. Here’s a concise breakdown:

* **Control Plane (Master Node)**: The control plane is responsible for managing the entire Kubernetes cluster, making scheduling decisions, and orchestrating workloads. It includes:

  * API Server: The main entry point to the Kubernetes cluster, handling all REST requests and exposing the Kubernetes API.
  * etcd: A key-value store that stores all cluster data, including configuration and state information.
  * Controller Manager: Monitors the cluster state and makes adjustments (e.g., scaling and replica management) to meet the desired state.
  * Scheduler: Assigns newly created pods to nodes based on resource availability, constraints, and policies.

* Worker Nodes: These nodes run the applications in pods and report to the control plane. Each worker node includes:

  * Kubelet: The main agent on each worker node that receives instructions from the API server and manages pod lifecycle.
  * Kube-proxy: Manages network rules on each node to enable communication within and outside the cluster.
  * Container Runtime: The software (e.g., Docker, containerd) responsible for running containers.
  
* Pods: The smallest deployable units in Kubernetes, representing a single instance of a running process. Pods can contain one or more tightly coupled containers.

![alt text](image.png)

*Note: If you would like to run a small Kubernetes service for testing you can run Minicube*

### What is a Cluster?

* Made up of at least 1 **Master Node** and 1 **Worker Node**.

A cluster in Kubernetes is a group of machines (nodes) that work together to run and manage containerized applications. It includes a control plane for orchestration and worker nodes for running workloads.

### Master vs. Worker Nodes

* **Master Nodes (works on the Control Plane)**: Manage the cluster, making scheduling and orchestration decisions, and maintain the desired state. They host components like the API server, controller manager, scheduler, and etcd.

* **Worker Nodes (work on the Data Plane)**: Run application workloads in containers within pods, managed by the control plane. Each worker has components like the kubelet, kube-proxy, and container runtime.

### Pros and Cons of Using Managed Service vs. Launching Your Own

* **Managed Service (e.g., GKE, EKS, AKS)**
  * **Pros**: Reduces operational overhead, automated updates and scaling, easy integrations, and high availability by default.
    * **Especially with Azure (AKS)**: Only charged for the Worker Nodes

  * **Cons**: Higher costs, limited control over infrastructure, and potential vendor lock-in.

* **Launching Your Own (Self-managed)**

*Recomended to have a minimum of 3 Master Nodes - Redundancy*

  * **Pros**: Full control over configuration, infrastructure choice, and customization; potentially lower costs at scale.
  * **Cons**: Requires significant operational knowledge, higher maintenance, and manual scaling and updates.

### Control Plane vs. Data Plane
* Control Plane: Manages the Kubernetes cluster, making decisions about scheduling, scaling, and maintaining the desired state.
* Data Plane: Executes the workloads (pods) on worker nodes, handling the actual application and network traffic.

### Kubernetes Objects
Kubernetes objects are persistent entities in the Kubernetes system that represent the desired state of resources and workloads in the cluster. Common ones include:

* **Pods**: The smallest deployable unit, representing mainly one (can more containers).
  * Pods have their own IP Adresses.
  * Pods are ephemeral as they are designed to be temporary.
* **Deployments**: Manages the deployment and scaling of pods, ensuring the desired number of replicas.
* **ReplicaSets**: Maintains a specified number of identical pod replicas for availability and resilience.
* **Services**: Exposes a set of pods as a network service, allowing external or internal access.
  * Handles/Worries the IP addresses to allow communication between pods.
* **ConfigMaps and Secrets**: Store configuration data and sensitive information, respectively, to be used by pods.
  * Secrets encrypt keys in base64 which easily decrypted.
* **Volume**: Important to store and persist data accross pods.
* **Namespace**: Logical way to group resources for an application.

![alt text](image-1.png)

### What Does it Mean a Pod is "Ephemeral"?
A pod is ephemeral because it is designed to be temporary and can be terminated, rescheduled, or replaced by Kubernetes at any time. Pods are not persistent and may be recreated on other nodes, leading to potential loss of state.

### How to Mitigate Security Concerns with Containers
* **Use Maintained Images**: Base images regularly updated by trusted sources reduce vulnerabilities.
* **Implement Image Scanning**: Use tools to scan images for vulnerabilities before deployment.
* **Limit Container Privileges (No-root privileges)**: Run containers with the least privileges necessary.
* **Use Network Policies**: Restrict pod-to-pod communication to only what’s required.
* **Regularly Update Dependencies**: Keep containers and dependencies updated.
* **Enable Runtime Security**: Use monitoring tools to detect unusual container behavior (logging).

### Maintained Images

**What Are They**: Pre-built container images managed and updated by trusted organizations or maintainers (e.g., official Docker or vendor-provided images).

**Pros**:
* Regular updates and security patches.
* Pre-configured with best practices.
* Reduced maintenance burden.
* Upto date documentation.

**Cons**:
* Limited customization and control.
* May include unnecessary components, increasing size.
* Dependency on maintainer updates and patch timing.

## Using Kubernetes

### Kubernetes Setup

1. Navigate to Docker Desktop
2. Navigate and select **Settings** > **Kubernetes** > **Enable Kubernetes** > **Apply and restart**

![alt text](image-2.png)

Check if Kubernetes is running `kubectl get all`

### Kubernetes Commands

Retrieves a list of all deployments in the current namespace, showing essential details like the deployment name, desired replicas, available replicas, and current status.

```bash
kubectl get deploy`
```
Retrieves all running resources in the current namespace, including pods, services, deployments, replica sets, daemon sets, and more. This provides an overview of 
all active components within the namespace.

```bash
kubectl get all`
```

Lists all pods in the current namespace, including information about each pod's status, restart count, and age.

```bash
kubectl get pods
```

Retrieves all replica sets in the current namespace, displaying the number of replicas and status information. Replica sets ensure that a specified number of pod replicas are running at any given time.

```bash
kubectl get replicasets
```

Creates the Kubernetes resource(s) specified in a YAML file, like pods, deployments, services, etc. This command only creates the resources if they do not already exist.

```bash
kubectl create -f <file.yaml>
```

Applies the configuration specified in a YAML file to an existing resource or creates it if it doesn’t exist. This command is ideal for making updates, as it compares the current configuration with the specified file and updates only the differences.

```bash
kubectl apply -f <file.yaml>
```

Deletes the Kubernetes resource(s) defined in the YAML file, removing the specific resource configurations and any associated pods, services, or other resources.

```bash
kubectl delete -f <file.yaml>
```

### Creating Objects: Deployments

1. Create a new file for the deployment

```bash
nano nginx-deployment.yml
```

Add the following yml code

```yml
---
apiVersion: apps/v1 # specify api to use for deployement
kind: Deployment # Kind of service/object to create
metadata:
  name: nginx-deployment # name the deployement
spec:
  selector:
    matchLabels:
      app: nginx # Look for this label/tag to match k8 service

  # Create a PeplicaSet with instances/pods
  replicas: 5
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: adonisdev/nginx-264
        ports: 
        - containerPort: 80
```

3. Check if the deployment object was created:

```bash
kubectl get deploy
```

### Creating Objects: Service

1. Creating a service to expose our pods to the outside world

*Node port service uses ports in this range 30000-32768*

2. Creating new file for the service

```bash
nano nginx-service.yml
```

Add the following yml code

```yml
---
apiVersion: v1
kind: Service
metadata:
  name: nginx-svc
  namespace: default
spec:
  ports:
  - nodePort: 30001
    port: 80
    targetPort: 80
  selector:
    app: nginx # Label to match service to deployment
  type: NodePort
```

3. Check if the service object was created:

```bash
kubectl get services
```

## Task: K8s deployment of NodeJS Sparta test app

![Kubernetes Architecture](Blank%20diagram%20(3).png)

### Creating the application

#### Step 1: Create the deployement

1. Create a project folder to deploy the kubernetes cluster

```bash
mkdir local-sparta-app-deploy
cd local-sparta-app-deploy
```

2. Create a file to provision the deployement

```bash
nano sparta-app-deploy.yml
```

Add the following contents:

```yml
---
apiVersion: apps/v1 # specify api to use for deployement
kind: Deployment # Kind of service/object to create
metadata:
  name: sparta-app-deployment # name the deployement
spec:
  selector:
    matchLabels:
      app: sparta-app # Look for this label/tag to match k8 service

  # Create a PeplicaSet with instances/pods
  replicas: 3
  template:
    metadata:
      labels:
        app: sparta-app
    spec:
      containers:
      - name: sparta-app
        image: adonisdev/tech264-prov-sparta-app-auto:v1
        ports: 
        - containerPort: 3000
        env:
          - name: DB_HOST
            value: "mongodb://mongodb-svc:27017/posts"
```

3. Run the script to create the deployement

```bash
kubectl create -f sparta-app-deploy.yml
```

#### Step 2: Create the Service

1. Create a project folder to provision the service for the kubernetes cluster

```bash
mkdir local-sparta-app-service
cd local-sparta-app-service
```

2. Create a file to provision the service

```bash
nano sparta-app-service.yml
```

Add the following contents:

```yml
---
apiVersion: v1
kind: Service
metadata:
  name: sparta-app-svc
  namespace: default
spec:
  ports:
  - nodePort: 30001
    port: 80
    targetPort: 3000
  selector:
    app: sparta-app # Label to match service to deployment
  type: NodePort
```

3. Run the script to create the service

```bash
kubectl create -f sparta-app-service.yml
```

### Creating the database

#### Step 1: Create the deployement

1. Create a project folder to deploy the kubernetes cluster

```bash
mkdir local-mongodb-deploy
cd local-mongodb-deploy
```

2. Create a file to provision the deployement

```bash
nano mongodb-deploy.yml
```

Add the following contents:

```yml
apiVersion: apps/v1 # specify api to use for deployement
kind: Deployment # Kind of service/object to create
metadata:
  name: mongodb-deployment # name the deployement
spec:
  selector:
    matchLabels:
      app: mongodb # Look for this label/tag to match k8 service

  # Create a PeplicaSet with instances/pods
  replicas: 1
  template:
    metadata:
      labels:
        app: mongodb
    spec:
      containers:
      - name: mongodb
        image: mongo:7.0.6
        ports:
        - containerPort: 27017
```

3. Run the script to create the deployement

```bash
kubectl create -f mongo-deploy.yml
```

#### Step 2: Create the Service

1. Create a project folder to provision the service for the kubernetes cluster

```bash
mkdir local-mongodb-service
cd local-mongodb-service
```

2. Create a file to provision the service

```bash
nano mongodb-service.yml
```

Add the following contents:

```yml
---
apiVersion: v1
kind: Service
metadata:
  name: mongodb-svc
  namespace: default
spec:
  ports:
  - protocol: TCP
    port: 27017
    targetPort: 27017
  selector:
    app: mongodb # Label to match service to deployment
  type: NodePort
```

3. Run the script to create the service

```bash
kubectl create -f mongodb-service.yml
```

### Step 3: Seeding the database: Method 1

1. Open the application deploy yml script in a text editor

```bash
nano <file.yaml>
```

2. Add the following line od code to seed the database: `command: ["sh", "-c", "node /usr/src/app/seeds/seed.js && exec npm start"]`

Example:

```bash
apiVersion: apps/v1 # specify api to use for deployement
kind: Deployment # Kind of service/object to create
metadata:
  name: sparta-app-deployment # name the deployement
spec:
  selector:
    matchLabels:
      app: sparta-app # Look for this label/tag to match k8 service

  # Create a PeplicaSet with instances/pods
  replicas: 3
  template:
    metadata:
      labels:
        app: sparta-app
    spec:
      containers:
      - name: sparta-app
        image: adonisdev/tech264-prov-sparta-app-auto:v1
        command: ["sh", "-c", "node /usr/src/app/seeds/seed.js && exec npm start"]
        ports:
        - containerPort: 3000
        env:
          - name: DB_HOST
            value: "mongodb://mongodb-svc:27017/posts"
```

3. Apply the file change to re-congigure all pods

```bash
kubectl apply -f <file.yaml>
```

### Step 3: Seeding the database (Manual Approach): Method 2

1. Connect to a pod via an interactive shell session

```bash
wimpty kubectl exec -it <node_name> -- sh
```

2. Run the seeding command

```bash
node seeds/seed.jscd
```

## Task: Create 2-tier deployment with PV for database

### Step 1: Create the Persistent Volume (PV)

1. Create a project folder to provision the Persistant Volume (PV) for the kubernetes cluster

```bash
mkdir local-pv-deploy
cd local-pv-deploy
```

2. Create a file to provision the PV

```bash
nano mongo-pv.yaml
```

3. Add the following yml code to provision the PV

```yml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: mongo-pv
spec:
  capacity:
    storage: 100Mi  # Adjust this as needed, don't allocate too much storage
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/data/mongo"  # Path on the host (use this only for local development)
```

4. Apply the file change to create the PV

```bash
kubectl create -f mongo-pv.yaml
```

### Step 2: Create the Persistent Volume Claim (PVC)

1. Create a project folder to provision the Persistant Volume Claim (PV) for the kubernetes cluster

```bash
mkdir local-pvc-deploy
cd local-pvc-deploy
```

2. Create a file to provision the PVC

```bash
nano mongo-pvc.yaml
```

3. Add the following yml code to provision the PVC

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mongo-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 100Mi  # Match this with the PV storage capacity
```

4. Run the file change to create the PVC

```bash
kubectl create -f mongo-pvc.yaml
```

### Step 3: Create the MongoDB Deployment with the PVC

Adjust the previous Database Setup to include the following pv & pvc references and run all remaining components.

```yaml
apiVersion: apps/v1 # specify api to use for deployement
kind: Deployment # Kind of service/object to create
metadata:
  name: mongodb-deployment # name the deployement
spec:
  selector:
    matchLabels:
      app: mongodb # Look for this label/tag to match k8 service

  # Create a PeplicaSet with instances/pods
  replicas: 1
  template:
    metadata:
      labels:
        app: mongodb
    spec:
      containers:
      - name: mongodb
        image: mongo:7.0.6
        ports:
        - containerPort: 27017
        
        # Added code for PV & PVC links
        volumeMounts:
            - mountPath: "/data/db"
              name: mongo-storage
      volumes:
        - name: mongo-storage
          persistentVolumeClaim:
            claimName: mongo-pvc
```

This setup should allow your MongoDB data to persist between pod or deployment restarts and confirm data persistence on the /posts page.

## Task: Research types of autoscaling with K8s

1. **Horizontal Pod Autoscaler (HPA)**

* Function: Scales the number of pods in a deployment, replica set, or stateful set based on CPU or memory usage or custom metrics.
* Metrics: Typically, HPA uses metrics like CPU or memory consumption by default. Custom metrics can also be configured.
* Example: If the CPU usage across all pods in a deployment reaches a specified threshold (e.g., 80%), HPA can scale out by adding more pods to distribute the load.

2. **Vertical Pod Autoscaler (VPA)**
  
* Function: Adjusts the resource requests (CPU and memory) for individual containers in pods, helping to right-size the pod's resources as workload demands change.
* Use Case: Useful for applications with unpredictable resource usage patterns, VPA can increase or decrease the allocated resources (e.g., CPU or memory) as needed without adding or removing pods.
* Behavior: Typically restarts the pod to apply the new resource limits, so it's often used in cases where downtime is acceptable.

3. **Cluster Autoscaler**
  
* Function: Scales the number of nodes in a Kubernetes cluster up or down based on the scheduling needs of pods.
* Usage: Works in tandem with cloud providers (like AWS, GCP, or Azure) to automatically provision or deprovision virtual machines (VMs) based on pending or underutilized pods.
* Example: If new pods can't be scheduled due to insufficient resources, Cluster Autoscaler can add a node to the cluster. Conversely, if nodes are underutilized, it can terminate them to save costs.

4. **KEDA (Kubernetes Event-Driven Autoscaling)**

* Function: Allows event-driven autoscaling for Kubernetes workloads.
* Use Case: Scales workloads based on external event sources or metrics like message queues (Kafka, RabbitMQ), databases, or custom events, rather than just traditional CPU/memory usage.
* Example: Scaling up pods in response to a spike in message counts in a queue (e.g., from an Azure Service Bus or AWS SQS) to handle the increased workload.
Choosing Autoscaling Strategies in Kubernetes

**For a comprehensive autoscaling setup, many organizations combine these autoscaling types:**

* HPA and Cluster Autoscaler often work together to respond to both pod-level and node-level scaling needs.
* KEDA can complement HPA by enabling scaling based on custom event triggers.

## Task: Use Horizontal Pod Autoscaler (HPA) to scale the app

### Step 1: Creating and configuring the Horizontal Pod Autoscaler (HPA)

1. Deployment Configuration (deployment.yaml)

    This configuration file creates a Deployment for the sparta-app, specifying the number of replicas, resource limits, and other essential details to control the application's deployment in the Kubernetes cluster.

    ```yml
    apiVersion: apps/v1  # Specifies the Kubernetes API version for deployment
    kind: Deployment     # Defines the object type as a Deployment
    metadata:
      name: sparta-app-deployment  # Names the deployment "sparta-app-deployment"
    spec:
      selector:
        matchLabels:
          app: sparta-app  # Matches pods with the label "app: sparta-app" to associate with this deployment

      # replicas: 3  # Number of pod replicas to start initially
      template:
        metadata:
          labels:
            app: sparta-app  # Labels the pod instances for service discovery and selection
        spec:
          containers:
          - name: sparta-app  # Names the container "sparta-app"
            image: adonisdev/tech264-prov-sparta-app-auto:v1  # Specifies the container image to deploy
            ports:
            - containerPort: 3000  # Exposes port 3000 on the container
            env:
            - name: DB_HOST
              value: "mongodb://mongodb-svc:27017/posts"  # Sets environment variable for the database connection
            resources:
              requests:
                cpu: 50m  # Minimum CPU guaranteed to each pod (50 millicores)
              limits:
                cpu: 250m  # Maximum CPU each pod can use (250 millicores)
    ```

    Explanation of Key Fields:

    * **replicas**: Sets the initial number of pod replicas to 3, providing fault tolerance and load distribution.
    * **resources.requests**: Requests a minimum of 50 millicores (50m) CPU per pod, ensuring each pod has enough resources to function under baseline load.
    * **resources.limits**: Limits CPU usage to 250 millicores (250m) per pod to prevent excessive CPU consumption, which helps in efficient autoscaling and resource allocation.
    * **env**: Sets environment variables, here DB_HOST for connecting to the MongoDB service within the cluster.

2. Horizontal Pod Autoscaler (HPA) Configuration (hpa.yml)

    This configuration file defines the Horizontal Pod Autoscaler for sparta-app-deployment, allowing Kubernetes to automatically scale the deployment based on CPU usage.

    ```yaml
    apiVersion: autoscaling/v2  # Specifies the Kubernetes API version for autoscaling
    kind: HorizontalPodAutoscaler  # Defines the object as an HPA
    metadata:
      name: sparta-app-hpa  # Names the HPA "sparta-app-hpa"
    spec:
      scaleTargetRef:
        apiVersion: apps/v1  # Specifies the API version of the target resource
        kind: Deployment  # Target type is a Deployment
        name: sparta-app-deployment  # Name of the deployment to scale, matching "sparta-app-deployment"
      minReplicas: 2  # Minimum number of pod replicas the HPA maintains
      maxReplicas: 10  # Maximum number of replicas the HPA can scale up to
      metrics:
        - type: Resource
          resource:
            name: cpu
            target:
              type: Utilization
              averageUtilization: 50  # Target average CPU utilization at 50%
    ```

    Explanation of Key Fields:

    * **scaleTargetRef**: Specifies the target deployment (sparta-app-deployment) to scale based on CPU metrics.
    * **minReplicas and maxReplicas**: Set minimum and maximum limits for scaling, with at least 2 replicas always active and scaling up to 10 replicas if needed.
    * **metrics.target.averageUtilization**: Sets a target CPU utilization of 50%. The HPA will try to maintain this average across all pods in the deployment, adding or removing replicas as CPU usage fluctuates.

### Step 2: Setup the Metrics server

The Metrics Server is essential in Kubernetes for gathering real-time CPU and memory usage data from nodes and pods across the cluster. This data enables components like the Horizontal Pod Autoscaler (HPA) to make informed scaling decisions based on actual resource usage, helping Kubernetes automatically adjust the number of pod replicas to meet demand. Without the Metrics Server, the HPA and kubectl top commands cannot access resource metrics, making it impossible to monitor or autoscale applications based on CPU or memory utilization.

1. Deploy Metrics Server

    ```bash
    kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
    ```

   * **Purpose**: This command installs the metrics-server in your Kubernetes cluster by applying the latest components file from the Kubernetes metrics-server repository.
   * **Explanation**: The metrics-server is a cluster-wide aggregator of resource usage data, like CPU and memory. It’s necessary for features such as the Horizontal Pod Autoscaler (HPA) to function, as it collects resource metrics from each node in the cluster.

2. Check Metrics Server Logs

    ```bash
    kubectl logs -n kube-system deployment/metrics-server
    ```

   * **Purpose**: This retrieves the logs for the metrics-server deployment.
   * **Explanation**: Viewing the logs can help diagnose any issues with metrics-server after deployment. Common issues include TLS errors, which can prevent metrics-server from gathering data from kubelets on the nodes.

3. Verify Metrics Server Pod Status

    ```bash
    kubectl get pods -n kube-system -l k8s-app=metrics-server
    ```

   * **Purpose**: This command checks the status of the metrics-server pods in the kube-system namespace.
   * **Explanation**: The -l k8s-app=metrics-server filter helps in locating the specific pods that are part of the metrics-server application. This ensures that the metrics-server is up and running correctly and can start collecting metrics data from the nodes.

4. Patch Metrics Server for Insecure TLS Communication

    ```bash
    kubectl patch deployment metrics-server -n kube-system --type='json' -p='[   {     "op": "add",     "path": "/spec/template/spec/containers/0/args/-",     "value": "--kubelet-insecure-tls"   } ]'
    ```

   * **Purpose**: This patches the metrics-server deployment to add the --kubelet-insecure-tls argument.
   * **Explanation**: The --kubelet-insecure-tls flag allows metrics-server to skip TLS certificate verification when connecting to the kubelets on each node. This may be necessary if there are certificate verification issues or if the kubelets don't have fully configured certificates. This command modifies the deployment to allow metrics-server to function in such environments.

### Step 3: Load Test the Application

**Note: To load test with Apache Benchmark (AB) on Windows, you need to use it through Windows Subsystem for Linux (WSL) or run it on a separate Linux-based environment, as AB cannot be directly installed on Windows.**

1. Set Up WSL (if not already installed)

    Open PowerShell as Administrator and run the following command to enable WSL:

    ```powershell
    wsl --install
    ```
2. Install Ubuntu (if not already installed):

    * Open the Microsoft Store and search for Ubuntu (or another preferred Linux distribution) to install it on WSL. Once installed, open the Ubuntu app to set it up.
    * Follow the on-screen instructions to create a username and password for the Linux environment.

3.  Install Apache Benchmark (ab) on WSL

      Run this command to ensure you're working with the latest package list:
      ```powershell
      sudo apt update
      ```

4. Install Apache2-utils:
  
      ```powershell
      sudo apt install apache2-utils
      ```
5. Verify the Installation:

    ```powershell
    ab -V
    ```
6. Run a Load Test with ab for the Kubernetes application.

    ```powershell
    ab -n 20000 -c 200 http://localhost:30003/
    ```

    * **Purpose**: This command uses Apache Benchmark (ab) to simulate a load test by sending 20,000 requests with a concurrency of 200 requests to the application running at localhost:30003.
    * **Explanation**: This load test helps in understanding how the application and the metrics-server handle heavy traffic, simulating real-world usage under high load. This test will help ensure the metrics-server is able to track and report on resource usage effectively under load.

### Step 4: Monitor the Kubernetes Cluster.

  While ab runs the load test, use these **kubectl** commands to watch how your Kubernetes application scales in real-time:

  * The following command is used to monitor the Horizontal Pod Autoscaler (HPA) in real-time.

    * `kubectl get hpa`: This part retrieves the current status of the Horizontal Pod Autoscaler for any deployments or pods that have an HPA configured.
    * `-w (watch)`: The -w flag tells kubectl to continuously watch for and display updates to the HPA status.

    ```bash
    kubectl get hpa -w
    ```

  * The following command is used to monitor the status of all pods in real-time.

    * `kubectl get pods`: This part lists the current status of all pods in the cluster (or in a specific namespace if specified).
    * `-w (watch)`: The -w flag makes this command continuously watch for changes to the pod status in real time.

    ```bash
    kubectl get pods -w
    ```

  ![alt text](image-3.png)


