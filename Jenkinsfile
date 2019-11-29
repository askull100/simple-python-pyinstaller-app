pipeline {
	agent none //1
	stages {
		stage('Build') { //2
			agent { 
				docker {
					image 'python:2-alpine' //3
 				}
 			}
 			steps {
 				sh 'python -m py_compile sources/add2vals.py sources/calc.py'

//4
 			}
 		}
		stage('Test') {
			agent {
				docker {
					image 'qnib/pytest'
				}
			}
			steps {
				sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
			}
			post {
				always {
					junit 'test-reports/results.xml'
				}
			}
		}
		stage('Deliver') {
			agent {
				docker {
					image 'cdrx/pyinstaller-linux:python2'
				}
			}
			steps{
				sh 'pyinstaller --onefile sources/add2vals.py'
			}
			post {
				success {
					archiveArtifacts 'dist/add2vals'
				}
			}
		}
 	}
}










