---
layout: post
title: 使用docker-compose 部署jenkins
author:  shefh
catalog: true
date:  2019-11-13
tags:
    - docker 
header-img: img/blog-0.jpg
---


## 部署脚本

```shell
version: '2'
services:
  jenkins:
    image: jenkinsci/blueocean:latest
    container_name: jenkins
    ports:
      - '8080:8080'
      - '50000:50000'
    volumes:
      - '/docker/volumes/jenkins:/var/jenkins_home'
      - '/var/run/docker.sock:/var/run/docker.sock'
    restart: always
```

## 插件安装

### Pipeline Utility Steps 插件
#### 插件说明
用于json xml 等各种文件的读取

#### 安装教程

```
https://jenkins.io/doc/pipeline/steps/pipeline-utility-steps/#readjson-read-json-from-files-in-the-workspace
```

### docker-build-step 插件

## pipeline 脚本

```
pipeline {
    environment {
        REGISTER_URL = 'xxxx'
        REGISTER_ID = 'docker-register-pwd'
        DAFEN_VERSION = ''
    }
    options {
        // 添加日志打印时间
        timestamps()
        // 设置全局超时
        timeout(time: 30, unit: 'MINUTES')
    }
    agent {
        docker {
            image 'xxx/node:1.0.0'
            registryUrl 'xxx'
            registryCredentialsId 'docker-register-pwd'
        }
    }
    stages {
        stage('Build Init') {
            steps {
                script {
                 
                    def props = readJSON file: './package.json'
                    echo "package.json 读取完成"
                    

                    DAFEN_VERSION = props['version']

                    echo "当前版本:${DAFEN_VERSION}, Build:${env.BUILD_NUMBER}"
                }
            }
        }
        stage('Build') { 
            steps {
                sh 'pwd'
                echo "${REGISTER_URL}"
                echo "开始打包"

                sh 'npm install' 

                sh 'npm run build:prod'

                echo "打包成功"
            }
        }

        stage('Docker') {
          steps {
            echo "准备打镜像"
            sh 'docker build -f ./src/docker/Dockerfile  -t xxx-app .'

            sh "docker tag xxx-app ${REGISTER_URL}/xxx/xxx-app:${DAFEN_VERSION}.${env.BUILD_NUMBER}"
            echo "准备推送镜像"
            sh "docker push ${REGISTER_URL}/xxx/xxx-app:${DAFEN_VERSION}.${env.BUILD_NUMBER}"

            echo "镜像推送成功"
          }
        }
    }
}
```
## 参考链接
* [Jenkins+Git+Maven+Docker持续集成、部署实践](https://biteeniu.github.io/ci-cd/jenkins-ci/)
* [官方文档](https://jenkins.io/zh/doc/tutorials/build-a-node-js-and-react-app-with-npm/)
* http://www.mydlq.club/article/8/
* http://k4nz.com/Software_Engineering/Continuous_Delivery/Jenkins/Pipeline_-_Jenkins/Plugins_and_Programming.html


