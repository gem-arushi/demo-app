pipeline {
    agent any

    tools {
        jdk 'JDK17'
    }

    options {
        skipDefaultCheckout(true)
        timestamps()
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                bat 'mvnw.cmd -B -DskipTests compile'
            }
        }

        stage('Unit Tests') {
            steps {
                bat 'mvnw.cmd -B test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo 'Running SonarQube analysis...'

                // Uncomment when SonarQube is configured
                /*
                withSonarQubeEnv('SonarQube') {
                    bat 'mvnw.cmd -B org.sonarsource.scanner.maven:sonar-maven-plugin:sonar'
                }
                */
            }
        }

        stage('Package JAR') {
            steps {
                bat 'mvnw.cmd -B package -DskipTests'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }

        stage('Deploy to Test Environment') {
            steps {
                echo 'Deploying application using Helm to Test Environment'

                // Actual command later
                /*
                bat 'helm upgrade --install demo-app helm/demo-app'
                */
            }
        }

        stage('User Acceptance Tests') {
            steps {
                echo 'Executing UAT test suite'

                // Example Selenium execution
                /*
                bat 'mvn test -Puat'
                */
            }
        }

        stage('Promote to Production via ArgoCD') {
            steps {
                echo 'Promoting application through ArgoCD'

                // Actual ArgoCD command later
                /*
                bat 'argocd app sync demo-app'
                */
            }
        }
    }

    post {

        success {
            echo 'Pipeline completed successfully.'
        }

        failure {
            echo 'Pipeline execution failed.'
        }
    }
}