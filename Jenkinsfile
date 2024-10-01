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
    }
}
