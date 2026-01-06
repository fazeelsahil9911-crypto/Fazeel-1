## Ansible Task - Install Nginx on pathnex server

- name: Install Nginx on Pathnex server
  hosts: all
  become: yes

  tasks:
    -  name: Install nginx
       yum:
         name: nginx
         state: present

##  Terraform Task - Create EC2 (c5.xlarge)

provider "aws" {
  region = "us-east-1"
}

resources "aws_instance" "pathnexEC2" {
  ami            = "ami-0abcd1234abcd1234"
  instance_type  = "c5.xlarge"

  tags = {
    Name = "Pathnex-EC2"
  }
}


## Kubernetes Task - Create Nginx Pod

apiVersion: v1
kind: Pod
metadata:
  name: pathnex-pod
spec:
  containers:
    - name: web
      image: nginx:latest
      ports:
        - containerPort: 80

##   Shell script - Print Date & Hostname 

#!/bin/bash
echo "Date" $(date)"
echo "Hostname: $(Hostname)"


# Git Integration
##     Jenkings Pipeline - Checkout Git 
You will learn how to **checkout a git repository** and List files.

pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url:https://github.com/pathnex/sample-java-app.git'
            } 
         }
         stage('List Files') {
             steps {
                 sh 'ls -la'
             }
         }
    }
}

## Gitlab CI - Checkout GIT
You will learn how to **checkout a git repository in Gitlab CI**.

stages:
  - checkout

git-checkout:
  stage: checkout
  script:
    - git clone https://github.com/pathnex/sample-java-app.git
    - cd sample-java-app
    - ls  -la

 
