pipeline {
    agent any
    tools {
        maven "Maven3"
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/iabhisheksaini01/salary-api.git'
            }
        }
        stage('Test') {
            steps {
                echo "Executing Java Unit Testing with Junit"
                sh 'mvn test'
            }
        }

        stage('Generate HTML Report') {
            steps {
                sh 'mvn surefire-report:report-only'
            }
        }
        
        stage('Archive Reports') {
            steps {
                junit '**/target/surefire-reports/*.xml'
                archiveArtifacts artifacts: 'target/**/*.jar, target/surefire-reports/*.*', fingerprint: true
            }
        }
    }
    post {
        success {
            emailext(
                to: 'sainiabhishek619@gmail.com',
                subject: "Build SUCCESS: ${currentBuild.fullDisplayName}",
                body: """Hey Abhishek, your Build has been succeeded.  

HTML Test Report is attached.  

You can also view the test reports and artifacts here:  
${env.BUILD_URL}artifact/

Check Jenkins: ${env.BUILD_URL}""",
                attachmentsPattern: 'target/site/surefire-report.html'
            )
        }
        failure {
            emailext(
                to: 'sainiabhishek619@gmail.com',
                subject: "Build FAILED: ${currentBuild.fullDisplayName}",
                body: """Please check, your build has failed.  

Logs and reports (if available) are attached.  

Check Jenkins: ${env.BUILD_URL}""",
                attachLog: true,
                attachmentsPattern: 'target/site/surefire-report.html'
            )
        }
    }
}
