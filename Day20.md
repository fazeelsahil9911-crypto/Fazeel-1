# Day 20 - DEBUG DAY

Fix these intentionally broken files:

##    Ansible - Broken

- name Install stuff
  hosts all
  tasks:
    -name: install
     yum:
       name httpd
       state present


##     Terraform - Broken

resource aws_instance "Bad" {
 ami "ami-123"
 instance_type c61.8xlarge
}


##    Kubernetes - Broken

apiVersion apps/v1
kind Deployment
metadata:
 name broken-app
spec:
replicas:3
selector:
 metchLabels:
   app: broken
template:
 metadata:
  lables:
   app: broken
spec:
 container:
  -name: app
   image nginx


# Secrets & Credentials

##    Jenkins Pipeline - Using Credentials
You will learn how to **use credentials securely in Jenkins**.

pipeline {
    agent any
    environment {
        INSTITUTE_NAME = "Pathnex"
    }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Pathnex/sample-java-app.git'
            }
        }
        stage('Use Secret') {
            steps {
                withCredentials([string(credentialsId: 'PATHNEX_API_KEY', variable: 'API_KEY')]) {
                    sh 'echo "Using secret $API_KEY for $INSTITUTE_NAME"'
                }
            }
        }
    }
}

##    GitLab CI - Using CI/CD Variables
You will learn how to **use GitLab CI variables securely**.

stages:
  - build

variables:
  INSTITUTE_NAME: "Pathnex"
  API_KEY: "PLACEHOLDER_SECRET"

build:
  stage: build
  image: alpine:latest
  script:
    - echo "Using secret $API_KEY for $INSTITUTE_NAME"
   
