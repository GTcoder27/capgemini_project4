pipeline {
agent any

```
stages {

    stage('Checkout') {
        steps {
            git 'https://github.com/GTcoder27/capgemini_project4'
        }
    }

    stage('Install JMeter') {
        steps {
            sh '''
            wget https://archive.apache.org/dist/jmeter/binaries/apache-jmeter-5.6.3.tgz
            tar -xvf apache-jmeter-5.6.3.tgz
            '''
        }
    }

    stage('Run Test') {
        steps {
            sh '''
            apache-jmeter-5.6.3/bin/jmeter -n -t test.jmx -l results.jtl -e -o report
            '''
        }
    }

    stage('Archive Report') {
        steps {
            archiveArtifacts artifacts: 'report/**', fingerprint: true
        }
    }
}
```

}
