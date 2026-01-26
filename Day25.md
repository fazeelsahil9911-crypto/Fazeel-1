# Day 25 - Lineinfile, Ket Pair, Multi-Container, File count

##    Ansible - Modify Existing Line

---
- name: Update SSH config for Pathnex
  hosts: all
  become: yes

  tasks:
    - name: Disable root login
      lineinfile:
        path: /etc/ssh/sshd_config
        regexp: "^PermitRootLogin"
        line: "PermitRootLogin no"


##   Terraform - EC2 Key Pair Output

resource "aws_key_pair" "PathnexKey" {
  key_name   = "Pathnexkey"
  public_key = file("~/.ssh/id_rsa.pub")
}

output "Pathnex_key_Name" {
  value = aws_key_pair.Pathnex.Key_name
}


##    Kubernetes - Multi-Container pod (Sidecar Logging)

apiVersion: v1
kind: pod
metadata:
  name: pathnex-sidecar
spec:
 containers:
   - name: app
     image: nginx
   - name: log-writer
     image: busybox
     command: ["sh", "-c", "while true; do echo log entry; sleep 5; done"]


##   Shell Script - Count Files in Directory

#!/bin/bash

DIR="/var/log"

COUNT=$(1s $DIR | wc -1)

echo "Total files in $DIR: $COUNT"


# Advanced Caching 

##     Jenkins Pipeline - Cache Dependencies
You will learn how to **cache Maven dependencies with realistic labels**.

pipeline {
    agent any
    environment {
        INSTITUTE_NAME = "Pathnex"
        TEAM = "DevOps"
        ENV = "staging"
    }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Pathnex/sample-java-app.git'
            }
       }
       stage('Build') {
           steps {
               sh '''
               if [ -d ".m2" ]; then
                   echo "Using cached dependencies for $INSITITUTE_NAME in $ENV by $TEAM"
               else
                   mvn clean compile
               fi
               '''
           }
      }
   }
}

##   GitLab CI - Cache Dependencies
You will learn how to **cache dependencies in GitLab CI with tags**.

stages:
  - build

variables:
  INSITITUTE_NAME: "Pathnex"
  TEAM: "DevOps"
  ENV: "staging"

build:
  stage: build
  image: maven:3.8.1-jdk-17
  cache:
    key: "${ENV}-maven-cache"
    paths:
      - .m2/repository
  script:
    - mvn clean complie
    - echo "Cached dependencies for $INSTITUTE_NAME in $ENV by $TEAM"
