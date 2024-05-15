# Workshop EC2, ECS and EKS


# Amazon EKS (Elastic Kubernetes Service)

Creating an Amazon EKS (Elastic Kubernetes Service) cluster using the AWS Management Console involves several steps, from configuring the cluster settings to launching worker nodes. Here's a step-by-step guide to help you get started with EKS:

# Step 1: Create IAM Roles
=> Create an IAM role for your EKS Cluster: This role allows EKS to manage resources on your behalf.
Go to the IAM Management Console.

=> Create a new role and select "EKS" from the service list, then "EKS - Cluster" for the use case.
Attach policies like AmazonEKSClusterPolicy.
Name the role (e.g., EKS-ClusterRole).

=> Create an IAM role for EKS Worker Nodes: This role is used by worker nodes to make AWS API calls on your behalf.
Repeat the process for the worker node role.
=> Select "EC2" as the service, then attach the AmazonEKSWorkerNodePolicy, AmazonEC2ContainerRegistryReadOnly, and AmazonEKS_CNI_Policy.
Name the role (e.g., EKS-WorkerNodeRole).
# Step 2: Create the EKS Cluster
=> Go to the EKS Service in the AWS Management Console:
Navigate to the EKS dashboard and click "Create Cluster".
=> Configure Cluster Settings:
=> Name your cluster: Choose a unique name for your cluster.
=> Kubernetes version: Select the version of Kubernetes you want to use.
=> Cluster Service Role: Select the IAM role created for EKS clusters (e.g., EKS-ClusterRole).
=> Configure networking:
VPC and Subnets: Select the VPC and subnets where your cluster should be deployed. Ensure the subnets are in different Availability Zones for high availability.
Security Group: Assign a security group or create a new one that allows communication within the cluster.
Cluster endpoint access: Configure public and/or private access to the Kubernetes API server.
Logging: Enable or disable logging for API server, audit, controller manager, and scheduler logs to CloudWatch.
=> Tags: Optionally, add tags to the cluster for organization and billing.
=> Review and Create: Review all settings and click => "Create" to start the cluster creation process.
# Step 3: Configure Worker Nodes
=> After your cluster is active, you need to add worker nodes:

=> Launch Worker Nodes:
Go back to the EKS dashboard, select your cluster, and go to the "Compute" tab.
=> Click "Add Node Group".
=> Name the node group and select the node IAM role created earlier (e.g., EKS-WorkerNodeRole).
Set node type, size, and count: Choose the instance type, disk size, and the number of instances.
=> Networking: Select the subnets for the nodes (ideally, the same subnets used for the cluster).
Scaling: Configure the scaling policies for your node group.
Review and Create: Check your configurations and create the node group.

# Step 4: Configure kubectl


=> aws eks update-kubeconfig --region <region> --name <cluster_name> to set the context to your new cluster.


# Step 5: Configure kubectl with Your EKS Cluster
=> aws eks --region ap-south-1 update-kubeconfig --name my-test

# Step 6: Deploy NGINX on EKS
=> Create a Deployment for NGINXCreate a YAML file named deployment.yaml:
=> kubectl apply -f nginx-deployment.yaml
Check the DeploymentTo see if the NGINX pods are running, use:
=> kubectl get pods
# Step 7: Expose NGINX Publicly Using a Service

Create a ServiceCreate another YAML file named 
service.yaml  

=> kubectl apply -f nginx-service.yaml
Check the ServiceIt takes a few minutes for the AWS Load Balancer to be set up. Check the status of your service and retrieve the public IP address:
=> kubectl get svc
# Step 8: Access NGINX
http://<EXTERNAL-IP>
