pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' //so that they can be downloaded later
            }
      } 

      stage('Unit Test - junit and jacoco') {
            steps {
              sh "mvn test"
            }
            post {
              always {
                junit 'target/surefire-reports/*.xml'
                jacoco execPattern: 'target/jacoco.exec'
              }
            }
      }
        stage('Docker build and push') {
            steps {
              script{
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'Docker') {
                        sh 'printenv'
                        sh 'docker build -t varunkumarmc/numeric-app:""$GIT_COMMIT"" .'
                        sh 'docker push varunkumarmc/numeric-app:""$GIT_COMMIT""'
                    }
              
               }
              }
           }
        }
        
            
  }
}