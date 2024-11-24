pipeline {
    agent any 

    stages {
        stage('01. Clone Repo') {
            steps {
                echo "Cloning the repository"
                git branch: 'main', credentialsId: 'shubhamdhole97', url: 'git@github.com:shubhamdhole97/av.git'
            }
        }
        stage('02. Clean') {
            steps {
                echo "Cleaning project"
                sh 'mvn clean'
            }
        }
        stage('03. Package') {
            steps {
                echo "Packaging project"
                sh 'mvn package'
            }
        }
        stage('04. Deploy to Nexus') {
            steps {
                echo "Deploying to Nexus"
                sh 'curl -v -u admin:admin --upload-file target/av-0.0.1-SNAPSHOT.jar http://localhost:8081/repository/shubham/com/vtech/av/0.0.1/av-0.0.1-SNAPSHOT.jar'
            }
        }
        stage('05. Verify Deployment') {
            steps {
                echo "Verifying Deployment"
                script {
                    try {
                        def nexusURL = "http://localhost:8081/repository/shubham/com/vtech/av/0.0.1/av-0.0.1-SNAPSHOT.jar"
                        def response = httpRequest(url: nexusURL, validResponseCodes: '200')
                        echo "Deployment verification response: ${response.content}"
                    } catch (Exception e) {
                        error "Deployment verification failed: ${e.message}"
                    }
                }
            }
        }
        stage('06. SCP to Docker Server') {
            steps {
                echo "SCP to Docker Server"
                script {
                    sshagent(credentials: ['my-ssh-key-id']) { // Ensure SSH Agent plugin is installed
                        try {
                            sh 'scp -i /path/to/key.pem target/av-0.0.1-SNAPSHOT.jar user@docker-server:/root/Java-CBCICD/target/'
                            echo "SCP completed successfully"
                        } catch (Exception e) {
                            error "SCP failed: ${e.message}"
                        }
                    }
                }
            }
        }
    }
}
