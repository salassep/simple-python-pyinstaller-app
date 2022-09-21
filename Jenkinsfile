node {
    docker.image('python:2-alpine').inside {
        stage('Build') {
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }
    docker.image('qnib/pytest').inside {
        stage('Test') {
            sh 'py.test --verbose sources/test_calc.py'
            input message: 'Lanjutkan ke tahap Deploy ? (Klik "Proceed" untuk melanjutkan)'
        }
    }
    stage('Deploy') {
        sh "docker run --rm -v \$(pwd):/sources} cdrx/pyinstaller-linux:python2 'pyinstaller -F add2vals.py'"
    }
}
