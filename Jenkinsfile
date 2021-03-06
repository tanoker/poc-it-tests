pipeline {
    agent any
    parameters {
        password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password')
    }
    stages {
		stage ('Clone') {
			steps {
				sh '''
                    if [ ! -d "integration-tests" ]
                    then
                        mkdir integration-tests
                    else 
                        ls -a
                    fi
                '''
                dir('integration-tests') {
                    git branch: 'master',
                        credentialsId: 'git_creds',
                        url: 'https://github.com/tanoker/poc-it-tests'
				}
			
                sh '''
                    if [ ! -d "system-api-one" ]
                    then
                        mkdir system-api-one
                    else 
                        ls -a
                    fi
                '''
                dir('system-api-one') {
                    git branch: 'master',
                        credentialsId: 'git_creds',
                        url: 'https://github.com/tanoker/poc-it-system-api-one'
				}
			
                sh '''
                    if [ ! -d "system-api-two" ]
                    then
                        mkdir system-api-two
                    else 
                        ls -a
                    fi
                '''
                dir('system-api-two') {
                    git branch: 'master',
                        credentialsId: 'git_creds',
                        url: 'https://github.com/tanoker/poc-it-system-api-two'
				}
			
                sh '''
                    if [ ! -d "process-api" ]
                    then
                        mkdir process-api
                    else 
                        ls -a
                    fi
                '''
                dir('process-api') {
                    git branch: 'master',
                        credentialsId: 'git_creds',
                        url: 'https://github.com/tanoker/poc-it-process-api'
				}
			}
		}
        stage ('Deploy') {
            steps {
                dir('system-api-one') {
                    sh '''
                          mvn deploy -DmuleDeploy -Dpassword=${PASSWORD}
                        '''
                }
                
                dir('system-api-two') {                    
                    sh '''
                          mvn deploy -DmuleDeploy -Dpassword=${PASSWORD}
                        '''
                }

                dir('process-api') {                    
                    sh '''
                          mvn deploy -DmuleDeploy -Dpassword=${PASSWORD}
                        '''
				}
            }
        }
		
		stage ('Test') {
			steps {
                dir('integration-tests') {
					sh '''
						  mvn test
						'''
                }
			}
		}
	}

	post {
		always {
			dir('system-api-one') {
				sh '''
					  mvn mule:undeploy -Dmule.artifact=target/*.jar -DmuleDeploy -Dpassword=${PASSWORD}
					'''
			}
			
			dir('system-api-two') {
				sh '''
					  mvn mule:undeploy -Dmule.artifact=target/*.jar -DmuleDeploy -Dpassword=${PASSWORD}
					'''
			}

			dir('process-api') {
				sh '''
					  mvn mule:undeploy -Dmule.artifact=target/*.jar -DmuleDeploy -Dpassword=${PASSWORD}
					'''
			}
		}
	}
}