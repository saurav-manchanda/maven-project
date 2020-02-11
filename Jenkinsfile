pipeline{

	agent any
	
	tools {
		maven 'MAVEN'
	}
	
	stages{
		stage("package"){
			steps{
				bat label: '', script: 'mvn clean package'
			}
			post{
				success{
					echo "Archive is generated successfully."
					echo "Archiving the artifact.."
					archiveArtifacts '**/*.war'
				}
				failure{
					echo "Something went wrong."
				}
			}
		}
		
		stage("Deploy to staging"){
			steps{
				build 'maven-webapp-deploy-to-staging'
			}
			post{
				success{
					echo "Deployed to staging successfully."
				}
				failure{
					echo "failed to deploye to staging."
				}
			}
		}
		
		stage("Deploy to production"){
			steps{
				timeout(time:1, unit: 'HOURS') {
					input 'Want to deploy application to production?'
				}
				build 'maven-webapp-deploy-to-production'
			}
			post{
				success{
					echo "Deployed to production successfully."
				}
				failure{
					echo "Failed to deploy to production."
				}
			}
		}
	}
}