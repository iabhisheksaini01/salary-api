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
        stage('Archive Reports') {
            steps {
                junit '**/target/surefire-reports/*.xml'
                archiveArtifacts artifacts: 'target/**/*.jar, target/surefire-reports/*.*', fingerprint: true
            }
        }
    }
    post {
        success {
            mail(
                from: 'jenikins-unit-testing',
                to: 'sainiabhishek619@gmail.com',
                subject: "Build SUCCESS: ${currentBuild.fullDisplayName}",
                body: """Hey abhishek your Build has been succeeded.  
Test Report link has been attached below 

You can view the test reports and artifacts here:  
${env.BUILD_URL}artifact/

Check Jenkins: ${env.BUILD_URL}"""
            )
        }
        failure {
            mail(
                from: 'jenikins-unit-testing',
                to: 'sainiabhishek619@gmail.com',
                subject: "Build FAILED: ${currentBuild.fullDisplayName}",
                body: """Please check your build has been failed 


You can view the test reports and artifacts here:  
${env.BUILD_URL}artifact/

Check Jenkins: ${env.BUILD_URL}"""
            )
        }
}
}
