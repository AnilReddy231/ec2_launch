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
            mail to: 'anilkumar231.g@gmail.com',
              subject: "The pipeline ${currentBuild.fullDisplayName} completed successfully.",
              body: "Build had been successfully completed"
        }
      failure {
              mail bcc: '', body: "<b>Example</b><br>\n\<br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "ERROR CI: Project name -> ${env.JOB_NAME}", to: "anilkumar231.g@gmail.com";
/*            mail to: 'anilkumar231.g@gmail.com',
              subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
              body: "Something is wrong with ${env.BUILD_URL}" */
    }
      unstable {
          echo 'Build is Unstable'
      }
  }
}
