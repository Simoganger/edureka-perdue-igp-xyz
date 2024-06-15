pipeline {
    agent any
    environment {
        PROJECT_ID = "xyz-technologies-88625635"
        ANSIBLE_SERVER = "ansible-server"
        REMOTE_DIRECTORY = "//opt//docker"
    }
    
    tools {
        maven 'maven'
        jdk 'java'
    }

    stages {
        stage('Checkout from GitHub') {
            steps {
                git 'https://github.com/Simoganger/edureka-perdue-igp-xyz'
            }
        }

        stage('Compile project') {
            steps {
                sh 'mvn compile'
            }
        }

        stage('Test project') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Build project') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Copy to Ansible'){
            steps {
                script {
                    sshPublisher(
                        continueOnError: false,
                        failOnError: true,
                        publishers: [
                            sshPublisherDesc(
                                configName: env.ANSIBLE_SERVER,
                                verbose: true, 
                                transfers: [
                                    sshTransfer(
                                        remoteDirectory: env.REMOTE_DIRECTORY, 
                                        sourceFiles: '**/*.war,**/*.yml,**/**.yaml,Dockerfile',
                                        execCommand: 'ansible-playbook /opt/docker/xyz-tech-playbook.yml', 
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
        }
    }
}