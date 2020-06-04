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
            #chmod +x ./scripts/test_sample.py
            #./tests/test_sample.py
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
            sh(script: ./tests/test_sample.py)
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
   }
}
