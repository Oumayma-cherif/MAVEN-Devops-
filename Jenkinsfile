pipeline {
    agent any
    tools {
        jdk 'JAVA_HOME' // Ensure that JAVA_HOME is set correctly in Jenkins
        maven 'MAVEN_HOME' // Ensure that M2_HOME is set correctly in Jenkins
    }
    stages {
        stage('Clone Repository') {
            steps {
                // Clone your GitHub repository
                git branch: 'main', // Use 'main' or 'master' based on your default branch
                    url: 'https://github.com/Oumayma-cherif/MAVEN-Devops-.git'
            }
        }
        stage('Build') {
            steps {
                // Build the project with Maven
                sh 'mvn clean package'
            }
        }
        stage('Publish to Nexus') {
            steps {
                script {
                    nexusArtifactUploader(
                        nexusUrl: 'http://localhost:8081', // URL de votre Nexus
                        repository: 'maven-releases', // Nom de votre dépôt dans Nexus
                        credentialsId: 'nexus-credentials', // ID des identifiants Jenkins pour Nexus
                        artifacts: [
                            [
                                artifactId: 'my-app', // ID de votre artefact
                                groupId: 'com.example', // Group ID de votre artefact
                                version: '1.0.0', // Version Release
                                classifier: '',
                                file: 'target/my-app-1.0.0.jar' // Chemin vers votre artefact JAR
                            ]
                        ],
                        nexusVersion: 'nexus3', // Utilisez 'nexus3' pour Nexus 3.x
                        protocol: 'http' // Protocole à utiliser
                    )
                }
            }
        }
    }
}
