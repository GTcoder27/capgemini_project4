pipeline {
agent any

triggers {
    githubPush()
}

stages {

    stage('Checkout') {
        steps {
            git branch: 'main', url: 'https://github.com/GTcoder27/capgemini_project4'
        }
    }

    stage('Install JMeter') {
        steps {
            bat '''
            powershell -Command "Invoke-WebRequest -Uri https://archive.apache.org/dist/jmeter/binaries/apache-jmeter-5.6.3.zip -OutFile jmeter.zip"
            powershell -Command "Expand-Archive jmeter.zip -DestinationPath ."
            '''
        }
    }

    stage('Run Test') {
        steps {
            bat '''
            rmdir /s /q report
            apache-jmeter-5.6.3\\bin\\jmeter.bat -n -t test.jmx -l results.jtl -e -o report
            '''
        }
    }

    stage('Archive Report') {
        steps {
            archiveArtifacts artifacts: 'report/**', fingerprint: true
        }
    }
}

}