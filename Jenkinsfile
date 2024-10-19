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

            }
        }
    }
}
