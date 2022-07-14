pipeline{
	agent any
	
	stages{
	
	    stage('Upload Artifacts to Nexus'){
			steps{
				nexusArtifactUploader artifacts: [
				[
				artifactId: 'mulepoc',
				classifier: '',
				file: 'target/mulepoc-1.0.0-SNAPSHOT-mule-application.jar',
				type: 'war'
				]
				], 
				credentialsId: 'nexus',
				groupId: 'com.mycompany',
				nexusUrl: 'localhost:8081/nexus/content/repositories/muleRepo/',
				nexusVersion: 'nexus3',
				protocol: 'http',
				repository: 'muleApplications',
				version: '1.0.0-SNAPSHOT'
			}
		}
		stage('Deploy to Dev'){
			environment{
                	anypoint_userName = credentials('anypoint_usrName')
			            anypoint_passwrd = credentials('anypoint_password')
            		}
			steps{
				bat 'echo "Deploy into Dev"'
				git branch: 'devlop', credentialsId: '266a2115-b2ac-4e4d-bb8d-312b7d7249d5', url: 'https://github.com/raghusantoshkumar/mulesoft.git'
				bat "mvn -Dmaven.test.failure.ignore-true clean deploy -DmuleDeploy -DskipTests -Dmule.version=4.4.0 -Danypoint.username=$anypoint_userName -Danypoint.password=$anypoint_passwrd -Denv=Sandbox -Dappname=mulePocJenkin -Dbusiness=concentrix -DvCore=Micro -Dworkers=1 -DaltDeploymentRepository=myinternalrepo::default::file:///C:/muleRepo"

			}
		}
		stage('Deploy to Stage'){
			environment{
                	anypoint_userName = credentials('anypoint_usrName')
					        anypoint_passwrd = credentials('anypoint_password')
            		}
			 input{
    				message "Do you want to proceed for Stage deployment?"
 			      }
			steps{
				bat 'echo "Deploy into Release Environment"'
				git branch: 'release', credentialsId: '266a2115-b2ac-4e4d-bb8d-312b7d7249d5', url: 'https://github.com/raghusantoshkumar/mulesoft.git'
				bat "mvn -Dmaven.test.failure.ignore-true clean deploy -DmuleDeploy -DskipTests -Dmule.version=4.4.0 -Danypoint.username=$anypoint_userName -Danypoint.password=$anypoint_passwrd -Denv=Sandbox -Dappname=mulePocJenkin -Dbusiness=concentrix -DvCore=Micro -Dworkers=1 -DaltDeploymentRepository=myinternalrepo::default::file:///C:/muleRepo"
			}
		}
	}
}
