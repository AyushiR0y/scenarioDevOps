pipeline {
    agent any

    environment {
        ARTIFACTORY_USER = credentials('artifactory-credentials')
        ARTIFACTORY_PASSWORD = credentials('artifactory-credentials')
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', credentialsId: 'github-pat', url: 'https://github.com/AyushiR0y/scenarioDevOps.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    sh './gradlew clean build'
                }
            }
        }

        stage('Publish to Artifactory') {
            steps {
                script {
                    sh '''
                        ./gradlew artifactoryPublish \
                        -Partifactory_user=$ARTIFACTORY_USER \
                        -Partifactory_password=$ARTIFACTORY_PASSWORD
                    '''
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    sh './gradlew test'
                }
            }
        }
    }

    post {
        success {
            echo 'Build completed successfully!'
        }
        failure {
            echo 'Build failed.'
        }
    }
}
