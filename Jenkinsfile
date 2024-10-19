pipeline {
    agent any
    tools {
        jdk 'JAVA_HOME' // Ensure that JAVA_HOME is set correctly in Jenkins
        maven 'MAVEN_HOME' // Ensure that M2_HOME is set correctly in Jenkins
    }
    stages {
        stage('Clone Repository') {
            steps {
                // Ensure there are no leading spaces in the URL
                git branch: 'main', // Use 'main' or 'master' based on your default branch
                    url: 'https://github.com/Oumayma-cherif/MAVEN-Devops-.git'
            }
        }
        stage('Compile Stage') {
            steps {
                // Execute the Maven command to clean and compile
                sh 'mvn clean compile'
            }
        }
        stage('Build') {
            steps {
                // Construire le projet avec Maven
                sh 'mvn package'
            }
        }
        stage('Publish to Nexus') {
            steps {
                nexusArtifactUploader artifacts: [
                    [
                        artifactId: 'my-app', // ID de votre artefact
                        classifier: '',
                        file: 'target/my-app-1.0-SNAPSHOT.jar', // Chemin vers votre artefact JAR
                        groupId: 'com.example', // Group ID de votre artefact
                        version: '1.0-SNAPSHOT' // Version de l'artefact
                    ]
                ],
                credentialsId: 'nexus-credentials', // ID des identifiants Jenkins pour Nexus
                nexusUrl: 'http://localhost:8081', // URL de votre Nexus
                nexusVersion: 'nexus3' // Utilisez 'nexus3' pour Nexus 3.x
            }
        }
    }
}
