pipeline {
    agent any 

    stages {
        stage('01.Clone Repo') {
            steps {
                echo "Git Cloning"
                 git branch: 'main', credentialsId: 'shubhamdhole97', url: 'git@github.com:shubhamdhole97/av.git'
            }
        }
        stage('02.Clean') {
            steps {
                echo "Cleaning"
                 sh 'mvn clean'
            }
        }
        stage('03.Package') {
            steps {
                echo "Packaging"
                 sh 'mvn package'
            }
        }
        stage('04.Deploy to Nexus'){
            steps{
                echo "Deploy to Nexus"
                 sh 'curl -v -u admin:admin --upload-file target/av-0.0.1-SNAPSHOT.jar http://localhost:8081/repository/shubham/com/vtech/av/0.0.1/av-0.0.1-SNAPSHOT.jar'                
            }
        }
        // (Optional) Verify the deployment by checking if the artifact exists in Nexus( HTTP request Plugins required )
        stage('05.Verify Deployment') {
            steps {
                echo "Verifying Deployment"
                 script {
                     def nexusURL = "http://localhost:8081/repository/shubham/com/msoffice/msexcel/0.0.1/msexcel-0.0.1-SNAPSHOT.jar"
                     def response = httpRequest(url: nexusURL, validResponseCodes: '200')
                     echo "Deployment verification response: ${response.content}"
                 }
            }
        }
        stage("06.SCP to Docker Server")
        {
            steps{
                echo "SCP To Docker Server"
                // script {
                //     sshagent(credentials: ['my-ssh-key-id']) {  // ssh-agent plugin required to install
                //         try {
                //             def result = bat(script: 'scp -v -i C:\\Users\\info\\.ssh\\mujahed.pem target\\msexcel-0.0.1-SNAPSHOT.jar pwd1:/root/Java-CBCICD/target/', returnStdout: true)
                //             echo "SCP command output: ${result}"
                //         }
                //         catch (Exception e) {
                //             echo "SCP failed: ${e.getMessage()}"
                //         }
                //     }
                // }
            }    
        }
    }
}
