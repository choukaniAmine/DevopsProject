pipeline {
    agent any
    triggers {
        githubPush()
    }
    tools {
        maven 'Maven3'
        jdk 'jdk17'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Compile') {
            steps {
                sh 'mvn clean compile'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'mvn test -Dtest=EnrollementManagementTests,DepartementsManagementTests,StudentManagementApplicationTests'
            }
        }
    }
    post {
        always {
            // Envoie un e-mail quel que soit le résultat
            emailext (
                subject: "Build ${currentBuild.fullDisplayName} - ${currentBuild.currentResult}",
                body: "Le build ${env.BUILD_NUMBER} pour ${env.JOB_NAME} est terminé avec le statut : ${currentBuild.currentResult}.\n\nConsultez la console : ${env.BUILD_URL}",
                to: "mohamedamienchoukani02@gmail.com"
            )
        }
    }
}
