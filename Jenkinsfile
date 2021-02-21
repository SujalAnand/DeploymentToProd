pipeline {
  agent any
  tools { 
        maven 'maven-3.6.3'
        jdk 'jdk8' 
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
    stage('Regression Testing') {
      steps {
	    echo "~~~~~~~Running Postman Scripts~~~~~~~~~"
        bat '"C:\\Users\\Administrator\\AppData\\Roaming\\npm\\"newman run "C:\\Users\\Administrator\\Desktop\\Postman_Collection\\CI-CD-GetFlights-Jenkins.postman_collection.json"  -r htmlextra --reporter-htmlextra-export "C:\\Users\\Administrator\\Desktop\\Postman_Collection" --reporter-htmlextra-darkTheme'
      }
    }
	
	stage('Release Jar to Jfrog') {
      steps {
	    echo "~~~~~~~Cutting a release in git as well as in Jfrog~~~~~~~~~"
		bat 'cd "C:\Windows\System32\config\systemprofile\AppData\Local\Jenkins\.jenkins\workspace\MuleSoft_DevOps"'
		bat 'mvn release:clean release:prepare release:perform -DskipStaging=true'
      }
    }
	
	stage('Download Jar from Jfrog') {
      steps {
	    echo "~~~~~~~Copying Jar From Jfrog~~~~~~~~~"
        bat 'mvn dependency:copy -Dartifact="com.mycompany:cicd-demo-project:1.0.11:jar:mule-application" -DoutputDirectory="C:\\Users\\Administrator\\Desktop\\Jar"'
      }
    }
	stage('Deploying To Production') {
	environment {
        USER_CREDENTIALS = credentials('AnypointExchangeID')
       // muleEnv = "${env.cloudhub_env.toLowerCase()}"
      }
      steps {
	    echo "~~~~~~~Deployment to Production Environment~~~~~~~~~"
        bat 'anypoint-cli --username=%USER_CREDENTIALS_USR% --password=%USER_CREDENTIALS_PSW% runtime-mgr cloudhub-application deploy --environment "Prod" --runtime "4.3.0" --workers "1" --workerSize "0.1" --region "us-east-1" "Prod-ci-cd-demo-project" "cicd-demo-project-1.0.11-mule-application.jar" --property "mule.env:prod"'
		echo "~~~~~~~Describing the status Of API Deployed~~~~~~~~~"
        bat 'anypoint-cli --username=%USER_CREDENTIALS_USR% --password=%USER_CREDENTIALS_PSW% runtime-mgr cloudhub-application describe --environment "Prod" Prod-cicd-demo-project'
	  }
    }
  }
}
