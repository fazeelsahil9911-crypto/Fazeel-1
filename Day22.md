# Day 22 - Handlers, Security Group, DeamonSet, Disk Monitor

##    Ansible - Use Handler on Config Change 

---
- name: Configure Nginx for Pathnex
  hosts: all
  become: yes

  tasks:
    - name: Copy Nginx config
      copy:
        dest: /etc/nginx/nginx.conf
        content: |
          user nginx;
          worked_processes auto;
      notify: Restart nGINX

  handlers:
    - name: Restart nginx
      service:
        name: nginx
        state: restarted


##    Terraform - Security Group

resource "aws_security_group" "Pathnex56" {
  name   = "pathnex-5g"
  vpc_id = aws_vpc,PathnexVPC.id

  ingress {
    from_port   = 80
    to_port     = 80
    protovol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
   }

   tags = {
     Name = "Pathnex-SG"
   }
 }


 ##  Kubernetes - DeamonSet

 apiVersion: apps/v1
 kind: DeamonSet
 metadata:
   name: pathnex-deamon
 spec:
   selector:
     matchLabels:
       app: ds
   template:
     metadata:
       labels:
         app: ds
     spec:
       containers:
         - name: monitor
           image: busybox
           command: ["sh", "-c", "while true; do echo Pathnex Node Monitor; sleep 20; done"]


##     Shell Script - Monitor Disk Usage

#!/bin/bash

USAGE=$(df -h | tail -1 awk '{print $5}')

echo "Disk Usage: $USAGE"


# Advanced Triggers 

##    Jenkins Pipeline - Trigger on Git Tag
You will learn how to **trigger builds on Git tags** with realistic tags.

pipeline {
    ageny any 
    environment {
        INSTITUTE_NAME = "Pathnex"
        Team = "DevOps"
        ENV = "prod"
    }
    triggers {
        pollSCM('H/15 * * * *')
    }    
    stages {
       stage('Checkout') {
           steps {
              git url: 'https://github.com/Pathnex/sample-java-app.git', bRANCH: 'main'
           }
       }
       stage('Build') {
           when {
               buildingTag()
           }
           steps {
               sh 'echo "Building $INSTITUTE_NAME app for $TEAM team is $ENV environment"'
           }
       }
   } 
}

##   GitLab CI - Trigger on Tag
You will learn how to **trigger CI jobs when a tag is pushed**.

stages:
  - build

variables:
  INSTITUTE_NAME: "Pathnex"
  TEAM: "DevOps"
  ENV: "prod"

build:
  stage: build
  image: maven:3.8.1-jdk-17
  script:
    - echo "Building $INSTITUTE_NAME app for $TEAM in $ENV environment"
  only:
    - tags
  
