node {
    // Pemicu (Triggers) DITEMPATKAN DI LUAR stage
    triggers {
        pollSCM('H/2 * * * *') // Memeriksa perubahan SCM setiap 2 menit
    }
    stage('Checkout') {
        checkout scm
    }
    stage('Build') {
        docker.image('python:3.9-alpine').inside('-u root') {
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }
    stage('Test') {
        docker.image('qnib/pytest').inside('-u root') {
            sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
        }
        junit 'test-reports/results.xml'
    }
}