---
- hosts: webservers
  become: yes

  tasks:
    - apt: update_cache=yes

    - apt:
        name: nginx
        state: present

    - template:
        src: nginx.conf.j2
        dest: /etc/nginx/nginx.conf
      notify: restart nginx

  handlers:
    - name: restart nginx
      service:
        name: nginx
        state: restarted
pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/user-name/repo.git'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean package -DskipTests'
            }
        }
    }
}
@echo off

copy "%WORKSPACE%\target\*.jar" "D:\Deployment\"

echo Deployment Successful
