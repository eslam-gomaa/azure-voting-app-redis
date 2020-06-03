pipeline {
   agent any

   stages {
      stage('Verify Branch') {
         steps {
            echo $"GIT_BRANCH"
         }
      }
      stage('Hello 2') {
         steps {
            echo 'Step in the Second Stage'
         }
      }
   }
}
