---
layout: post
title: "CI/CD pipeline with docker and kubernetes"
date: 2020-04-14 20:10:00 +0300
tags: Jenkins, Docker , Kubernetes
category: Tutorial
author: Poonam Agarwal
---

### setup Jenkins
$ mkdir jenkins
$ cd jenkins
$ vim Vagrantfile

	Vagrant.configure("2") do |config|
	  config.vm.box = "bento/ubuntu-20.04"
	  config.vm.network "forwarded_port", guest: 8080, host: 8081, host_ip: "127.0.0.1"
	  config.vm.network "private_network", ip: "192.168.10.31"

	  config.vm.provision "shell", inline: <<-SHELL
	    sudo apt-get update
	    sudo apt-get -y upgrade
	    sudo apt install openjdk-11-jdk -y  
	    wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
	    echo "deb https://pkg.jenkins.io/debian-stable binary/" | sudo tee -a  /etc/apt/sources.list > /dev/null
	    sudo apt-get update
	    sudo apt-get install -y jenkins
	  SHELL
	end

$ vagrant up
$ vagrabt ssh

### Setup Repository for docker

$ sudo apt-get update
$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common


$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
$ sudo apt-key fingerprint 0EBFCD88

$ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"


### Install docker engine

$ sudo apt-get update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io

#### updated user jenkins to run docker 
$ sudo usermod -G docker jenkins	
$ sudo apt-get install acl

$ sudo setfacl -m user:jenkins:rw /var/run/docker.sock

### Setup Kubernetes

$ curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl

$ chmod +x ./kubectl
$ sudo mv ./kubectl /usr/local/bin/kubectl

$ cp -r ~/.kube /var/lib/jenkins/
$ sudo chown -R jenkins:jenkins /var/lib/jenkins/
apiVersion: v1
clusters:
- cluster:
    certificate-authority: /var/lib/jenkins/.minikube/ca.crt
    server: https://192.168.99.100:8443
  name: minikube
contexts:
- context:
    cluster: minikube
    user: minikube
  name: minikube
current-context: minikube
kind: Config
preferences: {}
users:
- name: minikube
  user:
    client-certificate: /var/lib/jenkins/.minikube/profiles/minikube/client.crt
    client-key: /var/lib/jenkins/.minikube/profiles/minikube/client.key

make sure the permission of the following files should be as follows
client.crt 644, 
client.key 600, 
ca.crt 644
and folder /var/lib/jenkins/.minikube must have user as jenkins user


$ Adding docker hub credentials in jenkins
Credentials → System → Global Credentials → Add Credentials

<div>

<figure>
<img src="{{ site.github.url }}/media/img/credentials.png" />
<figcaption>Login Details</figcaption>
</figure>

</div>

Job → Pipeline → Poll SCM(after every push it will auto build the job)

<div>

<figure>
<img src="{{ site.github.url }}/media/img/pollscm.png" />
<figcaption>Poll Scm</figcaption>
</figure>

</div>


	❖ Add the groovy script
		➢ either via copy
		➢ or via SCM
	❖ Apply → Save → Build
	❖ Test on command line
		➢ kubectl get svc
		➢ http://192.168.10.30:<< port >>/hello/deepak

$ The groovy script:

pipeline {
   agent any
   environment {
       dockerHub = "docker.io"
       docker_cred = 'dockerhub'
   }
   stages {
		stage('SCM Checkout'){
			steps{
				checkout([$class: 'GitSCM',branches: [[name: 'origin/master']],extensions: [[$class: 'WipeWorkspace']],userRemoteConfigs: [[url: 'https://github.com/agrawalpoonam/python-app.git']]  ])
			}
		}
		stage("Build Docker Image"){
			steps{
				sh 'pwd'
				sh "docker build -t poonamag/test-app . --no-cache"
			}
		}
		stage('Upload Image to DockerHub'){
			steps{ 
	     	    withCredentials([
     		 	[$class: 'UsernamePasswordMultiBinding', credentialsId: docker_cred, usernameVariable: 'dockeruser', passwordVariable: 'dockerpass'],
  				]){
					sh "docker login -u ${dockeruser} -p ${dockerpass} ${dockerHub}"
  				}
	    	  	sh 'docker push poonamag/test-app'
	    	 }
	  	}
		stage("Running Docker Image"){
			steps{
				sh "kubectl get namespaces"
				sh "kubectl apply -f deployment.yaml"
			}
		}
}
}


