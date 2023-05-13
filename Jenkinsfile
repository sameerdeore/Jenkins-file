pipeline {

         agent {
		      
			  label {
			  
			          label "built-in"
					  customWorkspace "/mnt/game-of-life"
			        }
			 }
			  tools {
			          maven "maven-3.9.1"
			         }
			  
         stages {
		 
		        stage ("Delete Workspace Before Build") {
			    
			    steps {
				        sh "rm -rf *"
				      }
			      }
		     
		      stage ("checkout SCM") {
			    
			    steps {
				        sh git "https://github.com/chitrangsawant/game-of-life.git"
				      }
			     }
	 
	          stage ("Build-stage") {
			    
			    steps {
				        sh "mvn clean install -DskipTests=true"
						sh "cp -r /mnt/game-of-life/gameoflife-web/target/gameoflife.war /efs-jenkins/"
				      }
			 }
			 
	          stage ("on-slave-1") {
			   agent {
			     label {
			 
			        label "slave-1"
					customWorkspace "/mnt/game-of-life"
	            }
			 }
			 
				steps {
				       sh "sudo cp -r /efs-jenkins/gameoflife.war /mnt/project/"
				       sh "sudo docker system prune -a --volumes -f"
				       sleep 10
				       sh "sudo docker-compose up -d --scale tomcat=2"
				       sh "sudo docker-compose ps"
				      }
					  
			}
				 
				 stage ("on-slave-2") {
			 agent {
			      label {
			        label "slave-2"
					customWorkspace "/mnt/game-of-life"
	             }
			 }
				steps {
				       sh "sudo cp -r /efs-jenkins/gameoflife.war /mnt/project/"
					   sh "docker system prune -a --volumes -f"
				       sleep 10
				       sh "docker-compose up -d --scale tomcat=2"
				       sh "docker-compose ps"
				      }
					  
			        }       
			}
			post {
				 always {
				       sh "docker-compose down"
				       sleep 10
				       sh "docker system prune -a --volumes -f"
				       }             
                    }
			
	}
