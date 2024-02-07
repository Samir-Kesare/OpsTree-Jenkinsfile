pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                script {
                    // Define the DSL for creating a Freestyle job
                    def jobDSL = '''
                        job('My-Freestyle-Job') {
                            description('This is a sample Freestyle job created using a Declarative Pipeline')
                            steps {
                                shell('echo "Hello, world!"')
                            }
                        }
                    '''
                    // Execute the job DSL to create the Freestyle job
                    jobDsl scriptText: jobDSL
                }
            }
        }
    }
    
    post {
        success {
            slackSend(color: '#36a64f', message: "Declarative Job completed successfully!", channel: "#jenkins", teamDomain: "demoworkspace-6868926", tokenCredentialId: "e96c6c7f-1fdf-4c4a-80fd-5ad178092678")
        }
        failure {
            slackSend(color: '#ff0000', message: "Declarative Job failed!", channel: "#jenkins", teamDomain: "demoworkspace-6868926", tokenCredentialId: "e96c6c7f-1fdf-4c4a-80fd-5ad178092678")
        }
    }
}
