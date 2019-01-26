pipeline {
    agent { label 'Controller' }
    options {
        buildDiscarder(
            // Only keep the 10 most recent builds
            logRotator(numToKeepStr:'3'))
            skipDefaultCheckout()
    }
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
              sh '/usr/bin/ansible-playbook ansible-ec2-provision.yml -vv > Build.log'
              }
      	}
        script{
          build_log = readFile('Build.log').trim()
          }
    }

/*    stage('Build Result') {
        steps {
              script{
                build_log = readFile('Build.log').trim()
                }
              echo "${build_log}"
            }
        } */
  }
  }
  post {
      always {
          echo 'One way or another, Pipeline had finished executing'
          echo "${build_log}"
          }
      success {
            mail to: 'anilkumr231.g@gmail.com',
              subject: "The pipeline ${currentBuild.fullDisplayName} completed successfully."
              body: "Build had been successfully completed"
        }
      failure {
        mail to: 'anilkumr231.g@gmail.com',
             subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
             body: "Something is wrong with ${env.BUILD_URL}"
    }
      unstable {
          echo 'Build is Unstable'
      }
  }
}
