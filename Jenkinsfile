pipeline {
    agent {
        docker {
            image 'maven:3-alpine' 
            args '-v /root/.m2:/root/.m2' 
        }
    }
    stages {
		stage("ssh-agent"){
			steps {
				script {
				sshagent (credentials: ['59d5c062-6674-4484-9669-7136eeea6336']) {
				sh 'ssh -o StrictHostKeyChecking=no -l amit 192.168.1.109 uname -a'
					}
				}
			}
		}
    stage('Build') { 
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
        }
    stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
    stage('Deliver') { 
            steps {
                sh './jenkins/scripts/deliver.sh' 
            }    
        }
    }
}
