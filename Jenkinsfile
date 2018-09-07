pipeline {

    agent any	
		
	tools {
		maven 'MAVEN_HOME'
	}	
	
    stages('Sequential') {  
	
		stage('SCM'){										
			steps {		
			    
				echo 'Check-out'
				echo '=================================================='
				echo 'cuser=: ${params.cuser}'
				echo 'cpwd=: ${params.cpwd}'
				echo 'prjname=: ${params.prjname}'
				echo 'DEV_ENV=: ${params.DEV_ENV}'
				echo 'SAND_ENV=: ${params.SAND_ENV}'
				echo '=================================================='
				
				git 'https://github.com/akash-prajapati/labmulemaven.git'		
			}
		}			
		
		stage('DEV'){							
			steps {				
				bat 'mvn clean package deploy -Dcloudhub.username=${cloudhub.username} -Dcloudhub.password=${cloudhub.password} -Dcloudhub.domain=${cloudhub.domain}-dev -P dev'		
		    }
		}
		
		stage('WAIT_FOR_USER_INPUT') {
         			
			input {
                message "Let\'s promote?"
                ok "yes"
                submitter "admin"
                parameters {
                    string(name: 'response', defaultValue: 'yes', description: 'Go Ahead')
                }
            }
			steps {
				echo "Promote the code."
			}
        
		}
		
		stage('SANDBOX'){	
							
			steps {						
				bat 'mvn clean package deploy -Dcloudhub.username=${cloudhub.username} -Dcloudhub.password=${cloudhub.password} -Dcloudhub.domain=${cloudhub.domain}-sandbox -P sandbox'
			}
		}
	}
}
