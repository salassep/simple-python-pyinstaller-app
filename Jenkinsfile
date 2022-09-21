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
    docker.image('cdrx/pyinstaller-linux:python2').inside {
        stage('Deploy') {
            sh 'pyinstaller sources/add2vals.py'
            sleep 60s
        }
    }
}
