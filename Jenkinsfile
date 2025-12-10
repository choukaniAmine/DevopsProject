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
           stage('SonarQube Analysis') {
            steps {
                echo "🔍 Running SonarQube analysis..."
                // 'sonar' must match the name of your SonarQube server in Jenkins
                withSonarQubeEnv('sonar') {
                    sh """
                        mvn sonar:sonar \
                          -Dsonar.projectKey=chouka \
                          -Dsonar.projectName='DevopsProject-main' \
                          -Dsonar.java.binaries=target/classes \
                          -Dsonar.sources=src/main/java \
                          -Dsonar.tests=src/test/java \
                          -Dsonar.coverage.jacoco.xmlReportPaths=target/site/jacoco/jacoco.xml \
                          -Dsonar.test.inclusions='**/EnrollmentManagementTests.java,**/DepartementsManagementTests.java,**/StudentManagementApplicationTests.java'
                    """
                }
            }
        }
    }
    post {
        always {

            emailext (
                subject: "Build ${currentBuild.fullDisplayName} - ${currentBuild.currentResult}",
                body: "Le build ${env.BUILD_NUMBER} pour ${env.JOB_NAME} est terminé avec le statut : ${currentBuild.currentResult}.\n\nConsultez la console : ${env.BUILD_URL}",
                to: "mohamedaminechoukani02@gmail.com"
            )
        }
    }
}
