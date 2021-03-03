pipeline {
  agent any
    
  tools {nodejs "nodejs"}
    
  stages {
        
    stage('Git') {
      steps {
        git 'https://github.com/desinenisuresh/nodejs-project'
      }
    }
     
    stage('Build') {
      steps {
	    sh 'pwd'
        sh 'npm install'
        sh '<<Build Command>>'
        sh 'npm install express'
      }
    }  
    
            
    stage('Test') {
      steps {
        sh 'node test'
      }
    }
	
	stage('Server Instantion') {
      steps {
        sh 'node start'
      }
    }
	/*
	stage('Code Quality Check via SonarQube') {
      steps {
       script {
        def scannerHome = tool 'sonarqube';
           withSonarQubeEnv("sonarqube-container") {
           sh "${tool("sonarqube")}/bin/sonar-scanner \
           -Dsonar.projectKey=test-node-js \
           -Dsonar.sources=. \
           -Dsonar.css.node=. \
           -Dsonar.host.url=http://13.232.105.188:9000 \
           -Dsonar.login="43a1864f821bc940fdd77cfdad00477295026e6"
               }
           }
       }
   }
	*/
	
	 stage("Build image") {
            steps {
                script {
                    myapp = docker.build("desinenisuresh/hello:${env.BUILD_ID}")
                }
            }
        }
        stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'desinenisuresh') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }

		
	
  }
}
