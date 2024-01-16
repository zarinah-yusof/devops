pipeline{
    agent{
        label "jenkins-slave"
    }
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }
    environment {
        APP_NAME = "devops-project"
	RELEASE = "1.0.0"
	DOCKER_USER = "sitizarinahmohdyusof"
	DOCKER_PASS = 'docker-hub'
	IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
	IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
    }
    
    stages{
        stage("Cleanup Workspace"){
            steps {
                cleanWs()
            }

        }
    
        stage("Checkout from SCM"){
            steps {
                git branch: 'main', credentialsId: 'zarinah-yusof', url: 'https://github.com/zarinah-yusof/devops'
            }

        }

        stage("Build Application"){
            steps {
                sh "mvn clean package"
	    }
	}

	stage("Test Application"){
            steps{
	        sh "mvn test"
	    }
	}

        stage("Build & Push Docker Image") {
            steps {
                script {
                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image = docker.build "${IMAGE_NAME}"
                    }

                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
                    }
                }
            }

        }
    }

}
