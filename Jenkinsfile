pipeline { 

  environment { 

      Docker_Hub = "annaarakeyan/case-study-project" 

      Docker_Hub_Credential = 'dockerhub_id' 

      dockerImage = ''

  }

  agent any 

  stages { 

      stage('Clean workspace'){
				steps{
					script{
						sh 'rm -rf $PWD/2020_03_DO_Boston_casestudy_part_1'						
					}
				}
			}
      stage('Cloning Git Repo') { 

          steps { 

              sh 'git clone https://github.com/AnnaArakelyan909/2020_03_DO_Boston_casestudy_part_1.git'

          }

      } 

      stage('Building Docker image') { 

          steps { 

              script { 

                  checkout scm
                  sh 'docker image build -t $Docker_Hub:latest .'
                  sh 'docker image tag $Docker_Hub:latest $Docker_Hub:$BUILD_NUMBER'

              }

          } 

      }

      stage('Deploy our image') { 

          steps { 

              script { 

                  docker.withRegistry( '', Docker_Hub_Credential ) { 
				  
					  sh 'docker push $Docker_Hub:$BUILD_NUMBER'
					  sh 'docker push $Docker_Hub:latest'
					  
                  }

              } 

          }

      } 

      stage('Cleaning up') { 

          steps { 

              sh "docker rmi $Docker_Hub:$BUILD_NUMBER" 

          }

      } 

  }

}