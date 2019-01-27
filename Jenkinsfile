pipeline {
    agent { label 'Controller' }
    options {
        buildDiscarder(
            // Only keep the 3 most recent builds
            logRotator(numToKeepStr:'3'))
            skipDefaultCheckout()
    }
/*    environment {
        build_log = ""
    }   */
    stages {

    	stage('Check Out from SCM') { // Get some code from a GitHub repository
      	  steps {
		        git 'https://github.com/AnilReddy231/ec2_launch.git'
	           }
   	}

	   stage('BuildingModule') {
	      steps {
          timeout(time: 5, unit: 'MINUTES') {
            retry(2){
            withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws_jenkins_user']]){
              script{
              build_log = sh(
              returnStdout: true,
              script: '''ansible-playbook --vault-password-file ~/.ssh/vault_password ansible-ec2-provision.yml -i inventory/ec2.py -v''').trim()
              }
              }
              }
      	}
    }

  }
  }
  post {
      always {
          echo 'One way or another, Pipeline had finished executing'
          }
      success {
          echo "The pipeline ${currentBuild.fullDisplayName} completed successfully."
          }
      failure {
          echo "Failed Pipeline: ${currentBuild.fullDisplayName} Something is wrong with ${env.BUILD_URL}"
    }
      unstable {
          echo 'Build is Unstable'
      }
  }
}
