pipeline {
   //agent any
   agent {
       label 'DockerCompose'
   }
   stages {
      stage('Verify Branch') {
         steps {
            echo "$GIT_BRANCH"
         }
      }
      stage('Docker Build') {
         steps {
            sh(script: 'docker images -a')
            sh(script: """
            cd azure-vote/
            docker images -a
            docker build -t jenkins-pipeline .
            docker images -a
            cd ..
            """)
         }
      }
      stage('Start test app') {
         steps {
            sh(script: 'docker images -a')
            sh(script: """
            docker-compose up -d
            chmod +x ./scripts/test_container.ps1
            ./scripts/test_container.ps1
            """)
         }
         post {
            success {
                echo "App started successfully :)"
            }
            failure {
                echo "App failed to start :("
            }
        }
      }
      stage('Run Tests') {
         steps {
            sh(script: """
            chmod +x ./tests/test_sample.py
            ./tests/test_sample.py
            """)
         }
      }
      stage('Stop test app') {
         steps {
            sh(script: """
            docker-compose down
            
            """)
         }
      }
      stage('Push container') {
         steps {
            echo "Workspace is $WORKSPACE"
            dir("$WORKSPACE/azure-vote") {
               script { // Run Groovy code as a single script instead of having to define it line by line
                  docker.withRegistry('https://index.docker.io/v1', 'DockerHub') { //DockerHub - the if for DockerHub credentials from Jenkins
                     def image = docker.build('darkenman/azure-voting-app-testing:latest')
                     image.push()
                  }
               }
            }
         }
      }
   }
}
