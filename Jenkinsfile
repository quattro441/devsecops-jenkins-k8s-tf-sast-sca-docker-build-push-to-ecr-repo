pipeline {
  agent any
  tools { 
        maven 'maven_3_9_11'
    }
/*
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=asgbuggywebapp -Dsonar.organization=asgbuggywebapp -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=932558e169d66a8f1d1adf470b908a46156f5844'
			}
    }
*/

	stage('RunSCAAnalysisUsingSnyk') {
            steps {		
				withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
					sh 'mvn snyk:test -fn'
				}
			}
    }

	stage('Build') { 
            steps { 
               withDockerRegistry([credentialsId: "quattrosec", url: ""]) {
                 script{
                 app =  docker.build("quattrosec")
                 }
               }
            }
    }

	stage('Push') {
            steps {
                script{
                    docker.withRegistry('http://242611730375.dkr.ecr.eu-north-1.amazonaws.com/', 'ecr:eu-north-1:aws-credentials') {
                    app.push("latest")
                    }
                }
            }
    	}
	    
  }
