pipeline {
   agent any

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
            cd zure-vote/
            docker images -a
            docker built -t jenkins-pipeline .
            docker images -a
            cd ..
            """)
         }
      }
   }
}
