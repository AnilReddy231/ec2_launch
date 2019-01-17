pipeline {
    agent { label 'Controller' }
    options {
        buildDiscarder(
            // Only keep the 10 most recent builds
            logRotator(numToKeepStr:'3'))
    }
    stages {

    	stage('Check Out from SCM') { // Get some code from a GitHub repository
      	  steps {
		git 'https://github.com/AnilReddy231/letstrytoplay.git'
	  }
   	}

	stage('BuildingModule') {
	      steps {

        	sh '/usr/bin/ansible-playbook ansible-ec2-provision.yml -vv > Build.log'

      	}
    }
}
}
