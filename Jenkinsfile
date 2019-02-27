pipeline {
    agent any
    environment {
        CC = 'clang'
    }
    stages {
        stage('Example') {
            environment {
                DEBUG_FLAGS = '-g'
            }
            steps {
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
