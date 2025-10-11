# ☸️ Day 17: Amazon EKS (Elastic Kubernetes Service) – Hands-On Labs

## 🎯 Objective
You will learn how to:
- Create an EKS Cluster using `eksctl`
- Configure `kubectl` to connect
- Deploy and expose a sample app
- Integrate with Amazon ECR
- Scale and manage workloads

---

## ⚙️ Lab 1: Install Prerequisites

### 🧭 Steps

1. **Install AWS CLI**
   ```bash
   sudo apt update
   sudo apt install awscli -y
   aws --version

**2. Install kubectl (Kubernetes CLI)**

curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/latest/bin/linux/amd64/kubectl
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
kubectl version --client

**3. Install eksctl (EKS CLI Tool)**

curl --silent --location "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
eksctl version

☁️ Lab 2: Create an EKS Cluster

🧭 Steps

1. Create a Cluster using eksctl

eksctl create cluster \
--name my-eks-cluster \
--region ap-south-1 \
--nodegroup-name linux-nodes \
--node-type t3.medium \
--nodes 2 \
--nodes-min 1 \
--nodes-max 3 \
--managed

💡 This takes ~15–20 mins to create

2. Verify the Cluster

   eksctl get cluster

3. Update kubeconfig

   aws eks update-kubeconfig --name my-eks-cluster --region ap-south-1

4. Check Connection

   kubectl get nodes
   kubectl get pods -A

✅ Expected Result:
Cluster is running with 2 EC2 worker nodes in the Ready state.

🧩 Lab 3: Deploy a Sample Application

🧭 Steps

1. Create a Deployment YAML

cat <<EOF > sample-deploy.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
EOF

2. Apply the Deployment

   kubectl apply -f sample-deploy.yaml

3. Verify Deployment

   kubectl get deployments
   kubectl get pods

✅ Expected Result:
Two NGINX pods running successfully inside the cluster.

🌐 Lab 4: Expose Application with a LoadBalancer
🧭 Steps

1. Expose Deployment

   kubectl expose deployment nginx-deployment \
   --type=LoadBalancer \
   --port=80

2. Check Service

   kubectl get svc

3. Wait for the EXTERNAL-IP to be assigned (may take a few minutes).

4. Visit the External IP in a browser:

   http://<external-ip>

✅ Expected Result:
NGINX default welcome page appears — meaning the app is live through AWS Load Balancer.

📦 Lab 5: Integrate EKS with Amazon ECR
🧭 Steps

1. Authenticate Docker to ECR

   aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin <your_aws_account_id>.dkr.ecr.ap-south-1.amazonaws.com

2. Create a New ECR Repository

   aws ecr create-repository --repository-name my-app-repo

3. Tag & Push Docker Image

   docker build -t my-app .
   docker tag my-app:latest <your_aws_account_id>.dkr.ecr.ap-south-1.amazonaws.com/my-app-repo:latest
   docker push <your_aws_account_id>.dkr.ecr.ap-south-1.amazonaws.com/my-app-repo:latest

4. Deploy the ECR Image to EKS
  Update your YAML:

containers:
  - name: my-app
    image: <your_aws_account_id>.dkr.ecr.ap-south-1.amazonaws.com/my-app-repo:latest

Then apply:

  kubectl apply -f sample-deploy.yaml

✅ Expected Result:
Your containerized image from ECR runs successfully inside EKS.

🔄 Lab 6: Scale Your Deployment

🧭 Steps

1. Increase Replicas

   kubectl scale deployment nginx-deployment --replicas=5

2. Verify Scaling

   kubectl get pods -o wide

✅ Expected Result:
EKS automatically schedules 5 pods across available worker nodes.

📉 Lab 7: Monitor with CloudWatch

🧭 Steps

1. Enable Control Plane Logging
Go to AWS Console → EKS → Select your cluster → Logging tab
Enable: api, audit, authenticator, controllerManager, scheduler

2. View Logs in CloudWatch
Navigate to:
  CloudWatch → Logs → Log Groups → /aws/eks/<cluster-name>/cluster

✅ Expected Result:
You can view control plane logs and monitor API calls.

🧹 Lab 8: Clean-Up

To avoid unnecessary AWS charges, delete all resources once done:

kubectl delete svc nginx-deployment
kubectl delete deployment nginx-deployment
eksctl delete cluster --name my-eks-cluster --region ap-south-1

✅ Expected Result:
All EKS resources are deleted, and your environment is clean.

🧭 Summary

| Lab | Topic              | Key Learning                           |
| --- | ------------------ | -------------------------------------- |
| 1   | Setup Tools        | Installed AWS CLI, kubectl, eksctl     |
| 2   | Create EKS Cluster | Built and verified a working EKS setup |
| 3   | Deploy App         | Launched sample NGINX deployment       |
| 4   | LoadBalancer       | Exposed app to the Internet            |
| 5   | ECR Integration    | Pulled image from private registry     |
| 6   | Scaling            | Increased replicas dynamically         |
| 7   | Monitoring         | Logged activity via CloudWatch         |
| 8   | Cleanup            | Removed cluster and resources safely   |











