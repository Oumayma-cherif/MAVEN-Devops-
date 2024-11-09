pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                // Build the Maven project
                sh 'mvn clean install'
                // List the contents of the target directory for debugging
                sh 'ls -la target'
            }
        }

        stage('Upload to Nexus') {
            steps {
                script {
                    try {
                        nexusArtifactUploader(
                            nexusVersion: 'nexus3',
                            protocol: 'http',
                            nexusUrl: 'localhost:8081',
                            groupId: 'com.example',
                            version: '1.0-SNAPSHOT',
                            repository: 'maven-releases',
                            credentialsId: 'nexus-credentials', // Use your Jenkins credential ID here
                            artifacts: [
                                [
                                    artifactId: 'my-app',
                                    classifier: '',
                                    file: 'target/my-app-1.0-SNAPSHOT.jar',
                                    type: 'jar'
                                ]
                            ]
                        )
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        error "Upload failed: ${e.message}"
                    }
                }
            }
        }
    }

   post {
    always {
        echo 'Cleaning up workspace...'
        cleanWs()
    }
    success {
        echo "Sending success email..."
        emailext subject: "Jenkins Build - Success",
                 body: """
                 The build was successful for job ${env.JOB_NAME} (#${env.BUILD_NUMBER}).
                 Check console output at ${env.BUILD_URL} to view the details.
                 """,
                 to: 'maymach1823@gmail.com', // Updated email address
                 from: 'maymach1823@gmail.com', // Updated email address
                 replyTo: 'maymach1823@gmail.com', // Updated email address
                 mimeType: 'text/html'
        echo "Success email sent."
    }
    failure {
        echo "Sending failure email..."
        emailext subject: "Jenkins Build - Failure",
                 body: """
                 Unfortunately, the build has failed for job ${env.JOB_NAME} (#${env.BUILD_NUMBER}).
                 Check console output at ${env.BUILD_URL} to view the details.
                 """,
                 to: 'maymach1823@gmail.com', // Updated email address
                 from: 'maymach1823@gmail.com', // Updated email address
                 replyTo: 'maymach1823@gmail.com', // Updated email address
                 mimeType: 'text/html'
        echo "Failure email sent."
    }
}

}
