pipeline {
    agent any
 environment {
        ENV_DATE = sh(script: 'date -u +"%Y%m%d%H"', returnStdout: true)
 }
   stages {
        stage('Hello') {
            steps {
                script{
                    sh"""
                 echo ${env.ENV_DATE}

        """
            }
        }
    }
}
}
