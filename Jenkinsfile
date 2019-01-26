pipeline {
    agent { label 'Controller' }
    options {
        buildDiscarder(
            // Only keep the 10 most recent builds
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
              script{
              build_log = sh(
              returnStdout: true,
              script: '''/usr/bin/ansible-playbook ansible-ec2-provision.yml''').trim()
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
          echo "The pipeline ${currentBuild.fullDisplayName} completed successfully.",
          }
      failure {
          echo "Failed Pipeline: ${currentBuild.fullDisplayName} Something is wrong with ${env.BUILD_URL}"
    }
      unstable {
          echo 'Build is Unstable'
      }
  }
}
