pipeline {

    agent any

    tools{
        maven 'maven-3.8.2'
    }

    stages{

        stage('CheckOutCode') {
            steps {
                git branch: 'QA', credentialsId: '81a7bc69-4217-42b5-abb9-2408dc88549f', url: 'https://github.com/kalyannkotni/maven-web-application.git'
            }
        }

        stage('Build') {
            steps {
                sh  "mvn clean package"
            }
        }

        stage('ExecuteSonarQubeReport') {
            steps {
                sh  "mvn clean sonar:sonar"
            }
        }

        stage('UploadArtifactsIntoNexus') {
            steps {
                sh  "mvn clean deploy"
            }
        }
        
        
        stage('DeployAppintoTomcatServer') {
            steps {
                sshagent(['dcb726d5-34f5-4c03-8506-2178037f778f']) {
                    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@3.12.108.85:/opt/tomcat9/webapps/"
                }
            }
        }
        /*
        stage('Build Docker Image') {
            steps {
                sh "docker build -t kalyankotni/maven-web-application ."
            }
        }
        
        stage('Deploy Docker Image') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub_idd', variable: 'dockerhubpwd')]) {
                    sh "docker login -u kalyankotni -p ${dockerhubpwd}"
                }
                sh "docker push kalyankotni/maven-web-application"
            }
        }
        
        stage('Deploy to k8') {
            steps {
                kubernetesDeploy (configs: 'mavenwebappdeployment.yaml', kubeconfigId: 'kubeconfigg')
            }
        }
        */
    }
}
