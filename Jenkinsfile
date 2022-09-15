Node {
    docker.image('python:2-alpine').inside {
        stage('Build') {
            steps {
                sh 'python -m py_compile sources/add2vals.py sources/calc.py'
            }
        }
        docker.image('qnib/pytest').inside {
            stage('Test') {
                steps {
                    sh 'py.test --verbose sources/test_calc.py'
                }
            }
        }
    }
}