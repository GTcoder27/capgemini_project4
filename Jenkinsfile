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
            if exist report rmdir /s /q report
            apache-jmeter-5.6.3\\bin\\jmeter.bat -n -t test.jmx -l results.jtl -e -o report
            '''
        }
    }

    stage('Performance Gate') {
        steps {
            script {
                def total = 0
                def failed = 0

                def lines = readFile('results.jtl').split("\\n")

                for (int i = 1; i < lines.length; i++) {
                    def cols = lines[i].split(",")

                    if (cols.length > 7) {
                        total++
                        if (cols[7].trim() == "false") {
                            failed++
                        }
                    }
                }

                def errorRate = total > 0 ? (failed * 100.0) / total : 0

                echo "Total Requests: ${total}"
                echo "Failed Requests: ${failed}"
                echo "Error Rate: ${errorRate}%"

                if (errorRate > 2) {
                    error "❌ Performance Gate Failed: Error rate > 2%"
                } else {
                    echo "✅ Performance Gate Passed"
                }
            }
        }
    }

    stage('Archive Report') {
        steps {
            archiveArtifacts artifacts: 'report/**', fingerprint: true
        }
    }
}

}
