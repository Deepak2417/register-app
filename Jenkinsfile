pipeline {
    agent { label 'Jenkins-Agent' }
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }
    stages{
        stage("Cleanup Workspace"){
            steps {
                cleanWs()
            }
        }
        stage("Checkout from SCM"){
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/DevopsGuy1997/register-app.git'
            }
        }
        stage("Build & Unit Test"){
            steps {
                // Use 'mvn verify' which includes all phases up to test
                sh "mvn clean verify" 
            }
        }
        stage("Test Reporting & Archive"){
            steps {
                echo "Publishing test results and archiving artifact..."
                
                // 1. Publish JUnit test results for Jenkins reporting
                junit '**/target/surefire-reports/*.xml' 
                
                // 2. Archive the built application artifact (e.g., the JAR file)
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
        stage("Deploy to Staging"){
            steps {
                // TODO: Replace this with your actual deployment logic
                sh "echo 'Starting deployment of the archived artifact...'"
                // Example: sh "scp target/*.jar user@staging-server:/opt/app/"
            }
        }
    }
    // Post-actions run after all stages, useful for cleanup or notifications
    post {
        always {
            echo "Pipeline finished. Status: ${currentBuild.result}"
        }
        failure {
            // Optional: Send an email notification on failure
            // mail to: 'devops-team@example.com', subject: "Pipeline Failed: ${env.JOB_NAME}"
        }
    }
}
