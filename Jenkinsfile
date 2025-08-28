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
            body: """Hello Abhishek,  

Your build has completed successfully.  
Please find the attached HTML test report for details.""",
            attachmentsPattern: 'target/site/surefire-report.html'
        )
    }
    failure {
        emailext(
            to: 'sainiabhishek619@gmail.com',
            subject: "Build FAILED: ${currentBuild.fullDisplayName}",
            body: """Hello Abhishek,  

Your build has failed.  
Logs and test report (if available) are attached for review.""",
            attachLog: true,
            attachmentsPattern: 'target/site/surefire-report.html'
        )
    }
}

