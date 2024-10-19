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
}
