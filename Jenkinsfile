pipeline {
    
    tools{
        jdk 'myjava'
        maven 'mymaven'
    }
    agent none

    stages {
        stage('CloneRepo') {
            agent any
            steps {
                git 'https://github.com/deepakvasisht/DevOpsClassCodes.git'
            }
        }
         stage('Compile') {
             agent {label 'linux_slave'}
            steps {
                git 'https://github.com/deepakvasisht/DevOpsClassCodes.git'
                sh 'mvn compile'
            }
        }
        stage ('CodeReview') {
            agent {label 'linux_slave'}
            steps {
                sh 'mvn pmd:pmd'
            }
        }
        stage ('UnitTest') {
            agent {label 'windows_slave'}
            steps {
                git 'https://github.com/deepakvasisht/DevOpsClassCodes.git'
                bat 'mvn test'
            }
        
        post {
            success {
                junit 'target/surefire-reports/*.xml'
            }
        }
        }
        stage ('MetricCheck') {
            agent any
            steps {
                 sh 'mvn cobertura:cobertura -Dcobertura.report.format=xml'
            }
        }
       stage ('Package') {
            agent {label 'windows_slave'}
            steps {
                git 'https://github.com/deepakvasisht/DevOpsClassCodes.git'
                bat 'mvn package'
            }
        }
    }
}
