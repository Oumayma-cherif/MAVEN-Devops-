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
            def imageWidth = '800px'
            def imageHeight = 'auto'
            def imageUrl = 'https://semaphoreci.com/wp-content/uploads/2020/02/cic-cd-explained.jpg'

            echo "Sending success email..."
            emailext subject: "Jenkins Build - Success",
                     body: """
                    body: """
            <html>
                <body>
                    <p>YEEEEY, The Jenkins job was successful.</p>
                    <p>You can view the build at: <a href="${BUILD_URL}">${BUILD_URL}</a></p>
                    <p><img src="${imageUrl}" alt="Your Image" width="${imageWidth}" height="${imageHeight}"></p>
                    <p>Console Log is attached.</p>
                </body>
            </html>
        """,  
                     to: 'oumayma.cherif@esprit.tn',
                     from: 'oumayma.cherif@esprit.tn',
                     replyTo: 'oumayma.cherif@esprit.tn',
                     mimeType: 'text/html'
            echo "Success email sent."
        }
        failure {
            def imageWidth = '800px'
            def imageHeight = 'auto'
            def imageUrl = 'https://miro.medium.com/v2/resize:fit:1400/format:webp/1*ytlj68SIRGvi9mecSDb52g.png'

            echo "Sending failure email..."
            emailext subject: "Jenkins Build - Failure",
                    body: """
            <html>
                <body>
                    <p>noooo , The Jenkins job Failed .</p>
                    <p>You can view the build at: <a href="${BUILD_URL}">${BUILD_URL}</a></p>
                    <p><img src="${imageUrl}" alt="Your Image" width="${imageWidth}" height="${imageHeight}"></p>
                    <p>Console Log is attached.</p>
                </body>
            </html>
        """,
                     to: 'oumayma.cherif@esprit.tn',
                     from: 'oumayma.cherif@esprit.tn',
                     replyTo: 'oumayma.cherif@esprit.tn',
                     mimeType: 'text/html'
            echo "Failure email sent."
        }
    }
}
