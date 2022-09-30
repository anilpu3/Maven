pipeline{
      agent any
      stages{
            stage('check out'){
                  steps{
                        sh "rm -rf Maven"
                        sh "git clone "https://github.com/anilpu3/Maven.git"
                  }
            }
      }
            stage('build'){
                  steps{
                        sh "pwd"
                        sh "ls"
                        sh "cd Maven"
		                  	sh 'mvn clean install -DskipTests'
                        sh "echo ${BUILD_NUMBER}"
                        sh "docker build -t namma-maven-image:${BUILD_NUMBER} ."
                  }
            }
post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

stage ('Push image to Artifactory') { // take that image and push to artifactory
        steps {
            rtDockerPush(
                serverId: "Namma-Jfrog",
                image: "namma-maven-image:${BUILD_NUMBER}",
                host: 'tcp://localhost:8082',
                targetRepo: 'libs-release-local', // where to copy to (from docker-virtual)
                // Attach custom properties to the published artifacts:
                properties: 'project-name=namma-project;status=stable'
            )
        }
    }