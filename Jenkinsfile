pipeline {
    environment {
        registry = "bently5050/speproject"
        registryCredential = 'dockerhub'
    }
  agent any

  stages {
    stage('Clean') {
        steps{
        logstash{
             sh 'mvn clean'
             echo "clean"
        }
        }
    }

    stage('Compile') {
        steps{
        logstash{
             sh 'mvn package'
             echo "compile"
        }
        }
    }
    stage('Test') {
        steps{
           logstash{
            sh 'mvn test'
            echo "test"
           }
        }
    }

    stage('Deploy Image to Docker Hub') {
        steps{
        logstash{
            script{

               dockerImage = docker.build registry + ":spe"
               docker.withRegistry( '', registryCredential){
                   dockerImage.push()
               }
            }
        }
        }
    }
     stage('Execute Rundeck job') {
             steps {
             logstash{
               script {
                 step([$class: "RundeckNotifier",
                       includeRundeckLogs: true,
                       jobId: "c119f635-8b0c-48ea-9d2c-118764767cf1",
                       rundeckInstance: "rundeck",
                       shouldFailTheBuild: true,
                       shouldWaitForRundeckJob: true,
                       tailLog: true])
               }
               }
             }
    }

  }
  }
