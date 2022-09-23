node {
    docker.image('python:2-alpine').inside {
        stage('Build') {
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
            stash(name: 'compiled-results', includes: 'sources/*.py*')
        }
    }
    docker.image('qnib/pytest').inside {
        stage('Test') {
            sh 'py.test --verbose sources/test_calc.py'
        }
    }
    stage('Manual Approval') {
        input message: 'Lanjutkan ke tahap Deploy ? (Klik "Proceed" untuk melanjutkan)'
    }
    stage('Deploy') {
        
        dir(path: env.BUILD_ID) {
            unstash(name: 'compiled-results')
            sh "docker run --rm -v \$(pwd)/sources:/src cdrx/pyinstaller-linux:python2 'pyinstaller -F add2vals.py'"
        }
        
        archiveArtifacts "${env.BUILD_ID}/sources/dist/add2vals" 
        
        sh "docker run --rm -v  \$(pwd)/sources:/src cdrx/pyinstaller-linux:python2 'rm -rf build dist'"
        sh """
            scp -v -o StrictHostKeyChecking=no -i /var/jenkins_home/submission-2-devops-instance.pem\
             -r \$(pwd)/sources/ \${env.BUILD_ID}/sources/dist/add2vals\
              ubuntu@ec2-13-212-172-197.ap-southeast-1.compute.amazonaws.com:/home/ubuntu/simple-python-app
        """
        sleep(60)
    }
}
