pipeline {
  agent any
  tools {
    maven 'maven-3.6.3'
    jdk 'jdk8'
  }
  environment {
    USER_CREDENTIALS = credentials('AnypointExchangeID')
    version = "${env.API_Version}"
    env = "${env.Environment}"
    muleVersion = "${env.MuleRuntime}"
    workers = "${env.CloudHub_Workers}"
    wSize = "${env.Worker_Size}"
    region = "${env.CloudHub_Region}"
    muleEnv = "${env.Cloudhub_Env.toLowerCase()}"
  }
  stages {
    stage('Initialize') {
      steps {
        bat '''
				echo % PATH %
				echo % M2_HOME %
            ''' 
      }
    }

    stage('Download Jar from Jfrog') {
      steps {
        echo "~~~~~~~Copying Jar From Jfrog~~~~~~~~~"
        bat 'mvn dependency:copy -Dartifact="com.mycompany:ci-cd-jenkins-mule:%version%:jar:mule-application" -DoutputDirectory="C:\\Users\\Administrator\\Desktop\\Jar"'
      }

      post {
        success {
          echo "...Download from Artifactory Succeeded for ${env.BUILD_VERSION}: ${currentBuild.currentResult}"
        }
        failure {
          echo "...Download from Artifactory Failed for ${env.BUILD_VERSION}: ${currentBuild.currentResult}"
        }
      }

    }

    stage('Deploying To Production') {
      steps {
        echo "~~~~~~~Deployment to Production Environment~~~~~~~~~"
        //bat '"C:\\Users\\Administrator\\AppData\\Roaming\\npm\\"anypoint-cli --username=%USER_CREDENTIALS_USR% --password=%USER_CREDENTIALS_PSW% runtime-mgr cloudhub-application deploy --environment "%env%" --runtime "%muleVersion%" --workers "%workers%" --workerSize "%wSize%" --region "%region%" "Prod-ci-cd-demo-project" "C:\\Users\\Administrator\\Desktop\\Jar\\ci-cd-jenkins-mule-%version%-mule-application.jar" --property "mule.env:%muleEnv%"'
        echo "~~~~~~~Describing the status Of API Deployed~~~~~~~~~"
        //bat '"C:\\Users\\Administrator\\AppData\\Roaming\\npm\\"anypoint-cli --username=%USER_CREDENTIALS_USR% --password=%USER_CREDENTIALS_PSW% runtime-mgr cloudhub-application describe --environment "%env%" Prod-ci-cd-demo-project'
      }

      post {
        success {
          echo "...Successfully Deploying to Anypoint Platform"
        }
        failure {
          echo "...Failed to deploy to Anypoint Platform"
        }
      }
    }

  }
}
