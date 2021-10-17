pipeline {
    agent{
    label 'worker' 
    }

    tools{
        nodejs "NodeJS"
    }

    stages {
        stage("Install"){
            steps{
                sh 'npm install --save-dev clean-css'
                sh 'npm install uglify-js -g'
            }
        }
        stage("Compress"){
            parallel {
                stage("uglifyjs"){
                    steps{
                        sh 'cat www/js/* | uglifyjs -o www/min/merged-and-compressed.js --compress'
                    }
                }
                stage("cleancss"){
                    steps{
                        sh 'cat www/css/* | cleancss -o www/min/merged-and-minified.css'
                    }
                }
            }
        }
        stage ('save'){
			steps{
				sh "tar --exclude=.git --exclude=www/css --exclude=www/js -czvf backup.tar.gz *" 
			}
		}
		stage("archive"){
		    steps{
		        archiveArtifacts artifacts: 'backup.tar.gz', fingerprint: true
		    }
		}
    }
    post{
		always {
			sh 'node --version'
		}  
	}
}
