pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                // Build the Maven project
                sh 'mvn clean install'
            }
        }
        stage('Upload to Nexus') {
            steps {
                nexusArtifactUploader(
                    nexusUrl: 'http://localhost:8081', // Your Nexus URL
                    nexusCredentialsId: 'nexus-credentials', // Jenkins credentials ID
                    groupId: 'com.example',
                    artifactId: 'my-app',
                    version: '1.0-SNAPSHOT',
                    packaging: 'jar',
                    files: [
                        [file: 'target/my-app-1.0-SNAPSHOT.jar', classifier: '', type: 'jar']
                    ]
                )
            }
        }
    }
}
