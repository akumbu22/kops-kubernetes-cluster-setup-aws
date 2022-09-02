# kops-kubernetes-cluster-configuration
# Landmark Technologies,  -    Landmark Technologies 
# Tel: +1 437 215 2483,   -     +1 437 215 2483 
# mylandmarktech@gaIL.com,  -    www.mylandmarktech.com 

# Setting up Kubernetes (K8s) Cluster on AWS Using KOPS

1.kops is a software use to create production ready k8s cluster in a cloud provider like AWS.

2. kOPS SUPPORTS MULTIPLE CLOUD PROVIDERS

3. Kops compete with managed kubernestes services like EKS, AKS and GKE

4. Kops is cheaper than the others.

5. Kops create production ready K8S.

6. KOPS create resources like: LoadBalancers, ASG, Launch Configuration, woker node Master node (CONTROL PLANE.

7. KOPS is IaaC

#!/bin/bash
# 1) Create Ubuntu EC2 instance in AWS

# 2a) create kops user
`` sh
 sudo adduser kops
 sudo echo "kops  ALL=(ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/kops
 sudo su - kops
 ```
 
 ## 2a) install AWSCLI
 ``sh
 sudo apt update -y
 sudo apt install unzip wget -y
 sudo curl https://s3.amazonaws.com/aws-cli/awscli-bundle.zip -o awscli-bundle.zip
 sudo apt install unzip python -y
 sudo unzip awscli-bundle.zip
 sudo ./awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws
 ```
 
# 3) Install kops software on ubuntu instance:

 	#Install wget if not installed
 	sudo apt install wget -y
 	sudo wget https://github.com/kubernetes/kops/releases/download/v1.22.0/kops-linux-amd64
 	sudo chmod +x kops-linux-amd64
 	sudo mv kops-linux-amd64 /usr/local/bin/kops
	
	
 
# 4) Install kubectl

 sudo curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
 sudo chmod +x ./kubectl
 sudo mv ./kubectl /usr/local/bin/kubectl
 aws s3 mb s3://nubonglegah.k8.local
 aws s3 ls

1akumbu- download/install and check kubectl version in Linux

curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
kubectl version --client

2akumbu- download/install kops

curl -Lo kops https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64
chmod +x kops
sudo mv kops /usr/local/bin/kops

3akumbu-Install or update the AWS CLI

curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

4akumbu-Create an IAM role from AWS Console

Go to aws iam -click users- add users- select Access key & Programmatic access
click next- click Attach existing policies directly
Add the policies below in  # 5)
create n download
Now configure awscli with the above
run aws configure 
run aws iam list-users (to confirm users)

5akumbu-create an S3 bucket

e.g aws s3 mb s3://akumbu18.k8s.local OR
aws s3api create-bucket --bucket (bucketname) --region (regionName)
aws s3 ls (to see all buckets)

6akumbu- set environmental variables for kops

vi .bashrc (and paste the below)
        export NAME=akumbu18.k8s.local 
	export KOPS_STATE_STORE=s3://akumbu18.k8s.local
 source .bashrc (to refresh)
 
 7akumbu- generate ssh keys
 
 run ssh-keygen
 Now create cluster confirguration
 kops create cluster --zones us-east-1b --master-size t2.medium --master-count 1 --node-size t2.medium --node-count=2 ${NAME} --cloud aws
 Now copy and run your kops update cluster command given
 run kops edit cluster
 
 


# 5) Create an IAM role from AWS Console or CLI with below Policies.

	AmazonEC2FullAccess 
	AmazonS3FullAccess
	IAMFullAccess 
	AmazonVPCFullAccess


Then Attach IAM role to ubuntu server from Console Select KOPS Server --> Actions --> Instance Settings --> Attach/Replace IAM Role --> Select the role which
You Created. --> Save.



# 6) create an S3 bucket Execute below commond in KOPS Server use unique bucket name if you get bucket name exists error.

	aws s3 mb s3://class21.k8s.local
	aws s3 ls
	
    ex: s3://nubong.k8s.local
     
	Expose environment variable:

    # Add env variables in bashrc
    vi .bashrc
	
	# Give Unique Name And S3 Bucket which you created.
	export NAME=akumbu25.k8s.local
	export KOPS_STATE_STORE=s3://akumbu25.k8s.local
	
	export NAME=akumbu18.k8s.local 
	export KOPS_STATE_STORE=s3://akumbu18.k8s.local
 
    source .bashrc
	
# 7) Create sshkeys before creating cluster

    ssh-keygen
 

# 8) Create kubernetes cluster definitions on S3 bucket

	kops create cluster --zones us-east-1b --networking weave --master-size t2.medium --master-count 1 --node-size t2.medium --node-count=2 ${NAME}
	
	kops create secret --name ${NAME} sshpublickey admin -i ~/.ssh/id_rsa.pub

# 9) Create kubernetes cluser

	 kops update cluster ${NAME} --yes

# 10) Validate your cluster(KOPS will take some time to create cluster ,Execute below commond after 3 or 4 mins)

	   kops validate cluster

# 11) connect to the master node
    sh -i ~/.ssh/id_rsa ubuntu@ipAddress
    ssh -i ~/.ssh/id_rsa ubuntu@3.90.203.23
# 11) To list nodes

	  kubectl get nodes 
 
# 12) To Delete Cluster

  sudo kops delete cluster --name=${NAME} --state=${KOPS_STATE_STORE} --yes  
   
====================================================================================================


13 # IF you want to SSH to Kubernetes Master or Nodes Created by KOPS. You can SSH From KOPS_Server

sh -i ~/.ssh/id_rsa ubuntu@ipAddress
ssh -i ~/.ssh/id_rsa ubuntu@3.90.203.23
  
``
