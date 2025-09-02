pipeline {
  agent any

  stages {
      
      stage('Build Artifact') {
        steps {
          bat "mvn clean package -DskipTests=true"
          archive 'target/*.jar' //so that they can be downloaded later
        }
      }   

      stage('Unit Tests - JUnit and JaCoCo') {
        steps {
          bat "mvn test"
        }
      }
    stage('SonarQube - SAST') {
	    steps {
		    withSonarQubeEnv('SonarQube') {
			    bat "mvn sonar:sonar -Dsonar.projectKey=kubernetes-devops-security -Dsonar.host.url=http://localhost:9000"
		    }
	   }

     stage('Vulnerability Scan - Docker ') {
      steps {
        sh "mvn dependency-check:check"
      }
      post {
        always {
          dependencyCheckPublisher pattern: 'target/dependency-check-report.xml'
        }
      }
    }

  }



    }
}
