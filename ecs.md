# AWS EC2 Container Service ECS

  * AWS EC2 Container Service (ECS) is a highly scalable, high performance container management service that supports Docker containers and allows running applications on a managed cluster of EC2 instances
  * ECS eliminates the need to install, operate, and scale the cluster management infrastructure.
  * ECS is a regional service that simplifies running application containers in a highly available manner across multiple AZs within a region
  * ECS helps schedule the placement of containers across the cluster based on the resource needs and availability requirements.
  * ECS allows integration of your own custom scheduler or third-party schedulers to meet business or application specific requirements.

## ECS Elements
### Containers and Images

   * Applications deployed on ECS must be architected to run in docker containers, which is a standardized unit of software development, containing everything that the software application needs to run: code, runtime, system tools, system libraries, etc.
   * Containers are created from a read-only template called an image.
   * Images are typically built from a Dockerfile, and stored in a registry from which they can be downloaded and run on your container instances.
   * ECS can be configured to access a private Docker image registry within a VPC, Docker Hub or is integrated with EC2 Container Registry (ECR)

### Task Definitions

   * Task definition is needed to prepare application to run on ECS
   * Task definition is a text file in JSON format that describes one or more containers that form your application.
   * Task definitions specify various parameters for the application, such as containers to use, their repositories, ports to be opened, and data volumes

### Tasks and Scheduling

   * A task is the instantiation of a task definition on a container instance within the cluster.
   * After a task definition is created for the application within ECS, you can specify the number of tasks that will run on the cluster.
   * ECS task scheduler is responsible for placing tasks on container instances, with several different scheduling options available

### Clusters

   * Cluster is a logical grouping of EC2 instances to run tasks using ECS
   * ECS downloads the container images from the specified registry, and runs those images on the container instances within your cluster.

### Container Agent

   * Container agent runs on each instance within an ECS cluster
   * Container Agent sends information about the instance’s current running tasks and resource utilization to ECS, and starts and stops tasks whenever it receives a request from ECS

### ECS vs Elastic Beanstalk

   * ECS helps in having a more fine-grained control for custom application architectures.
   * Elastic Beanstalk is ideal to leverage the benefits of containers but just want the simplicity of deploying applications from development to production by uploading a container image.
   * Elastic Beanstalk is more of an application management platform that helps customers easily deploy and scale web applications and services.
   * With Elastic Beanstalk, specify container images to be deployed, with the CPU & memory requirements, port mappings and container links.
   * Elastic Beanstalk abstracts the finer details and automatically handles all the details such as provisioning an ECS cluster, balancing load, auto-scaling, monitoring, and placing the containers across the cluster.
