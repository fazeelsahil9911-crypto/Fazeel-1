# Day 16 - Ansible Roles, DynamoDB, Statefulset, Service Check

##    Ansible - Create Role Stracture

# Students must create:
# roles/
#   pathnex/
#     tasks/main.yml
#     handlers/main.yml
#     templates/
#     vars/main.yml

Example task file:

---
- name: Install httpd
  yum:
    name: httpd
    state: present


  ##   Terraform - Dynamo Table

  resource "aws_dynamodb_table" "PathnexTable" {
    name           = "PathnexTrainingTable"
    billing_mode   = "PAY_PER_REQUEST"
    hash_key       = "10"

    attribute {
      name = "10"
      type = "5"
  }

  tags = {
    Name = "Pathnex-DDB"
  }
}


##    Kubernetes - Statefulset

apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: pathnex-db
spec:
  serviceName: "pathnex"
  replicas: 2
  selector:
    matchLabels:
      app: pathnex-db
  template:
    metadata:
      labels:
        app: pathnex-db
    spec:
      container:
        - name: db
          image: mysql
          env:
            - name: MYSQL_ROOT_PASSWORD
              value: "Pathnex123"


##    Shell Script - Check Service Running

#1/bin/bash

SERVICE="sshd"

if systemctl is-active --quiet $SERVICE; then
  echo "$SERVICE is running"
else
  echo "$SERVICE is NOT running"
fi


# Artifacts Promotion

##    Jenkins Pipeline - Promote Artifacts
You will learn how to **promote build artifacts from dev to staging**.

Pipeline {
    agrnt any
    environment {
        INSTITUTE-NAME = "Pathnex"
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Promote') {
            steps {
                input message: "Promote artifacts to staging?"
                sh 'echo "Promoting artifacts for $INSTITUTE_NAME"'
            }
        }
    }
}

##    GitLab CI - Artifacts Promotion
You will learn how to **promote artifacts manually using Gitlab CI manual job**.

stages:
  - build
  - promote

build:
  stage: build
  image: maven:3.8.1-jdk-17
  script:
    - mvn clean package
  artifacts:
    paths:
      - target/*.jar
    expire_in: 1 week

promote:
  stage: promote
  image: alpine:latest
  script:
    - echo "Promoting artifacts for Pathnex"
  when: manual
  
  
  
  
