pipeline {
    agent any
    environment {
        CC = 'clang'
    }
    parameters {
        string(name: 'Greeting', defaultValue: 'Hello', description: 'How should I greet the world?')
    }
    stages {
        stage('Example') {
            environment {
                DEBUG_FLAGS = '-g'
            }
            steps {
                echo "${params.Greeting} World!"
                echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
                sh 'printenv'
            }
        }
        stage('Build') {
            steps {
                echo 'Building..'
                sh 'make'
                sh 'printenv'
            }
        }
        stage('Test') {
            steps {
                /* `make check` returns non-zero on test failures,
                * Using `true` to allow the Pipeline to continue nonetheless
                */
                echo 'Testing..'
                sh 'make check'
            }
        }
        stage('Deploy') {
            when {
                expression {
                    currentBuild.result == null || currentBuild.result == 'SUCCESS'
                }
            }
            steps {
                echo 'Deploying....'
                sh 'make publish'
            }
        }
    }
}
