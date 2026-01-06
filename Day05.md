# Day 05 - Handlers,  User-Data, Resource Limits

##    Ansible - Handler to Restart Nginx

---
- name: Manage Nginx 
  hosts: all
  become: yes

  tasks:
    - name: update config file
      copy:
        src: inder.html
        dest: /usr/share/nginx/html/inder.html
      notify: Restart Nginx 

  handlers:
    - name: Restart Nginx
      service:
        name: nginx
        state: restarted


##    Terraform - EC2 with User Data (c6a.12xlarge)

resource "aws_instance" "PathnexEC2" {
  ami           = "ami-0abcd1234abc1234"
  instance_type = "c6.12xlarge"

  user_data = <<EOF
#!/bin/bash
yum install -y httd
systemctl start httd
EOF

  tags = {
    Name = "Pathnex-EC2"
  }
}


##    Kubernetes - Resource Limits

apiVersion: v1
kind: pod
metadata:
  name: pathnex-limited-pod
spec:
  container:
    - name: app
      image: nginx
      resources:
        limits:
          memory: "256Mi"
          cpu: "500m"


# Day 05 - Environment Variables

##  Jenkins Pipeline - Use Environment Variables
You will learn how to **use environment variables in Jenkins pipelines**

pipeline {
    agent any
    environment {
        INSTITUTE_NAME = "Pathnex"
    }
    stages {
        stage('Print") {
            steps {
                sh 'echo "welcome to $INSTITUTE_NAME DevOps Training"'
            }
         }
         stage('Build') {
             steps {
                 sh 'mvn clean package - Dinstitue.name=$INSTITUTE_NAME'
             }
         }
     }
]

## Gitlab CI - Use Environment Variables 
You will learn how to **use CI variables in GitLab pipelines**.

Stages:
  - build

variables:
  INSTITUTE_NAME: "Pathnex"

build:
  stage: build
  image: maven:3.8.1-jdk-17
  script:
    - git clone https://github.com/Pathnex/sample-java-app.git
    - cd sample-java-app
    - echo "Welcome to $INSTITUTE_NAME DevOps Training"
    - mvn clean package - Dinstitute.name=$INSTITUTE_NAME
