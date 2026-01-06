# Day 02 - Services & Tags 

##    Ansible -  Install & Enable Nginx

---
- name: Install and start Nginx on pathnex
  hosts: all
  become: yes

  tasks:
    - name: Install nginx
      yum:
        name: nginx
        state: Present

    - name: Enable nginx
      service:
        name: nginx
        state: started
        enabled: yes


##    Terraform - EC3 with Tags (r5.2xlarge)

provider "aws" {
  region =us-east-1"
}

resource "aws_instance" "pathnexEC2" {
  ami           = "ami-0abcd1234abcd1234"
  instance_type = "r5.2xlarge"

  tags = {
    Name        = "Pathnex-Server"
    Environment = "Training"
    Owner       =  "PathnexStudent"
  }
}


##    Kubernetes - Deployment with 2 replicas

apiVersion:  apps/v1
kind: Deployment
metadata:
  name: pathnex-deployment
spec:
  replicas: 2
  selector:
    matchLebels:
      app: pathnex-app
  template:
    metadata:
      labels:
        app:  pathnex-app
    spec:
      containers:
        - name: app
          image: nginx 
          ports:
            - containerport: 80


##   Shell Script - Disk usage 

#!/bin/bash
df -h


# Maven Build 
##    Jenkinks Pipeline - Maven Build
You will learn how to **complie, test, and package a java project** using maven.

pipeline {
    agent  any
    tools  {
        maven 'Maven-3.8.1'
        jdk 'JDK-17'
    }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Pathnex/sample-java-app.git'
            }
         }
         stage('complie') {
             steps {
                sh  'mvn clean compile'
             }
         }
         stage('Test') {
             steps {
                 sh 'mvn test'
             }
         }
         stage('package') {
             steps {
                 sh 'mvn package'
             }
         }
     }


     ##   Gitlab C1 -Maven Build 

     Stages:
      - biuld 
      - test
      - package 

     maven-build:
      stage:  build
      image:  maven:3.8.1-jdk-17
      script:
        - git clone https://github.com/pathnex/sample-java-app.git
        - cd sample-java-app
        -mvn clean compile

     maven-test:
       stage: test
       image: maven:3.8.1-jdk-17
       script:
         - cd sample-java-app
         - mvn test

     maven-package:
       stage: package 
       image: maven:3.8.1-jdk-17
       script:
         - cd sample-java-app
         - mvn package
         
  
