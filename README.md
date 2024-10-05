# CICD-k8s-jenkins-kind-cluster-using-make

Clone the repo to your machine
sudo git clone <project repo URL>

Since we will be using Make through out our installation we need to create a make file that will have all our instllation steps 

sudo vim Makefile
add inside the file 

```bash
#!/usr/bin/env make
.PHONY: install_jenkins install_docker install_kind install_kubectl create_kind_cluster

install_jenkins:
	@sudo wget -O /usr/share/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
	@echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
	@sudo apt update
	@sudo apt install -y fontconfig openjdk-17-jre
	@sudo apt install -y jenkins
	@sudo systemctl start jenkins
	@sudo systemctl status jenkins

install_docker:
	@sudo apt update && sudo apt install -y ca-certificates curl
	@sudo install -m 0755 -d /etc/apt/keyrings
	@sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
	@sudo chmod a+r /etc/apt/keyrings/docker.asc
	@echo "deb [arch=$$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $$(. /etc/os-release && echo $${VERSION_CODENAME}) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
	@sudo apt update
	@sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
	@sudo usermod -aG docker $USER
	@newgrp docker

install_kind:
	@sudo curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.22.0/kind-linux-amd64 # for mac os based replace amd64 with arm64
	@sudo chmod +x ./kind
	@sudo mv ./kind /usr/local/bin/kind

install_kubectl:
	@sudo snap install kubectl --classic

create_kind_cluster:
	@kind create cluster --name my_cluster
	@kubectl get nodes

```
now we can start calling our installations one by one 
make install_jenkins

Make sure to log into the Jenkins user before installing kind to run the cluster using this user before switching to user jenkins set its password

sudo passwd jenkins # enter the new password of jenkins twice

Also we need to make sure jenkins has sudo privileges and is included in docker group

sudo visudo #go to the last line add
jenkins ALL=(ALL:ALL) NOPASSWD: ALL

make install_docker
then ensuring jenkins is added to docker group

sudo usermod -aG docker jenkins
id jenkins

su - jenkins

make install_kind
make install_kubectl
make create_kind_cluster

