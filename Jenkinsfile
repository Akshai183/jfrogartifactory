//Declarative Pipeline
def VERSION='1.0.0'
pipeline {
    agent any
    // tools {
	//  maven 'apache-maven-3.6.3'
    // }
    environment {
        PROJECT = "WELCOME TO DEVOPS B183 BATCH - Jenkins Class"
        AZ_SUB_ID = "9ce91e05-4b9e-4a42-95c1-4385c54920c6"
        AZ_TEN_ID = "2b387c91-acd6-4c88-a6aa-c92a96cab8b1"
        BATCH = "B183"
    }
    stages {
        stage("Dev Tools Verification") {
            agent { label 'DEV' }
            steps {
                sh "mvn --version"
                sh "java -version"
            }
        }
        stage('Git checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Akshai183/jfrogartifactory.git'
            }
        }
        stage('Dev mvn clean') {
            steps { 
                sh "mvn clean"
            }
        }
        stage('Dev mvn test') {
            steps { 
                sh "mvn test"
            }
        }
        stage('Dev mvn package & install') {
            steps { 
                sh "mvn versions:set -DnewVersion=Dev-1.0.${BUILD_NUMBER}"
                sh "mvn package install"
                sh "rm -rf /home/ubuntu/.m2/settings.xml"
                sh "cp dev-settings.xml /home/ubuntu/.m2/settings.xml"
            }
        }
        stage('Dev mvn package & deploy') {
            steps {
                sh "mvn versions:set -DnewVersion=Dev-1.0.${BUILD_NUMBER}"
                sh "mvn package deploy"
            }
        }
        stage('Dev docker build') {
            steps { 
                sh "sudo docker build -t akshai183/akshai183spring:$BUILD_NUMBER ."
            }
        }
        stage('Dev Deploy Docker Image') {
            steps { 
                sh "sudo docker stop springbootapp || sudo docker ps"
                sh "sudo docker run --rm -dit --name springbootapp -p 8080:8080 akshai183/akshai183spring:$BUILD_NUMBER"
            }
        }
        stage('Dev Validate Deployment') {
            options {
               timeout(time: 3, unit: 'MINUTES') 
            }
            steps { 
                sh "sleep 30 && curl http://dev.akshaik8s.xyz:8080 || exit 1"
            }
        }
    }
}

//testing
