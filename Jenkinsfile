pipeline {
  agent any
  tools { 
        maven 'maven-3.6.3'
        jdk 'jdk8' 
  }
	environment {
            USER_CREDENTIALS = credentials('AnypointExchangeID')
       // muleEnv = "${env.cloudhub_env.toLowerCase()}"
      }
  stages {
    stage ('Initialize') {
            steps {
                bat '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                ''' 
       }
    }
  
	

	
	stage('Download Jar from Jfrog') {
      steps {
	    echo "~~~~~~~Copying Jar From Jfrog~~~~~~~~~"
        bat 'mvn dependency:copy -Dartifact="com.mycompany:ci-cd-jenkins-mule:1.0.0:jar:mule-application" -DoutputDirectory="C:\\Users\\Administrator\\Desktop\\Jar"'
      }
    }
	stage('Deploying To Production') {
      steps {
	    echo "~~~~~~~Deployment to Production Environment~~~~~~~~~"
            bat '"C:\\Users\\Administrator\\AppData\\Roaming\\npm\\"anypoint-cli --username=%USER_CREDENTIALS_USR% --password=%USER_CREDENTIALS_PSW% runtime-mgr cloudhub-application deploy --environment "Sandbox" --runtime "4.3.0" --workers "1" --workerSize "0.1" --region "us-east-1" "Prod-ci-cd-demo-project" "C:\\Users\\Administrator\\Desktop\\Jar\\ci-cd-jenkins-mule-1.0.0-mule-application.jar" --property "mule.env:dev"'
	    echo "~~~~~~~Describing the status Of API Deployed~~~~~~~~~"
            bat '"C:\\Users\\Administrator\\AppData\\Roaming\\npm\\"anypoint-cli --username=%USER_CREDENTIALS_USR% --password=%USER_CREDENTIALS_PSW% runtime-mgr cloudhub-application describe --environment "Sandbox" Prod-ci-cd-demo-project'
	  }
    }
  }
}
