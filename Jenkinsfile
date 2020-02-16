pipeline {
    agent none
	stages {
        stage('Build') {
            agent {
                docker {
                    image 'python:2-alpine'
                }
            }
            steps {
		deleteDir()
		sh 'python -m py_compile app/app.py'
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'qnib/pytest'
                }
            }
            steps {
		withEnv(["HOME=${env.WORKSPACE}"]) {
	        sh 'pip install --user -r app/requirements.txt'
		sh 'py.test --verbose --junit-xml test-reports/results.xml app/app_test.py' }
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
      
    }
}
