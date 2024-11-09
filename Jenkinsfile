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
            emailext subject: "Jenkins Build - ${currentBuild.currentResult}",
                     body: """
                     The build result for job ${env.JOB_NAME} (#${env.BUILD_NUMBER}) is ${currentBuild.currentResult}.
                     Check console output at ${env.BUILD_URL} to view the details.
                     """,
                     to: 'oumayma.cherif@esprit.tn', // Replace with recipient's email
                     from: 'oumayma.cherif@esprit.tn', // Your email
                     replyTo: 'oumayma.cherif@esprit.tn', // Your email
                     mimeType: 'text/html'
        }
    }
}
