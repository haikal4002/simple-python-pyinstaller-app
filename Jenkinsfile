node {
    checkout scm
    stage('Build') {
        docker.image('python:3.9-alpine').inside('-u root') {
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }
    stage('Test') {
        docker.image('qnib/pytest').inside('-u root') {
            sh 'py.test --verbose --junit-xml test-reports/results.xml ./sources/test_calc.py'
        }
        junit 'test-reports/results.xml'
    }
    stage('Manual Approval'){
        input message: 'Lanjutkan ke tahap Deploy?'
    }
    stage('Deploy'){
        checkout scm
        docker.image('ubuntu:20.04').inside('-u root') {
            sh 'apt update'
            sh 'apt install python3 -y'
            sh 'apt install pip -y'
            sh 'pip install pyinstaller'
            sh 'pyinstaller --onefile sources/add2vals.py'
            archiveArtifacts artifacts: 'dist/add2vals', allowEmptyArchive: true
        }
        echo "Sleep for 1 minute"
        sleep time: 60, unit: "SECONDS"

    }
}
