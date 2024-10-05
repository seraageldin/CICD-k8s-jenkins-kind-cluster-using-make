# CICD-k8s-jenkins-kind-cluster-using-make

Clone the repo to your machine
sudo git clone <project repo URL>

Since we will be using Make through out our installation we need to create a make file that will have all our instllation steps 

sudo vim Makefile
add inside the file 
#!/usr/bin/env make
.PHONY: install_jenkins

install_jenkins:
    sudo wget -O /usr/share/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt update sudo apt install fontconfig openjdk-17-jre -y sudo apt install -y jenkins sudo systemctl start jenkins sudo systemctl status jenkins


