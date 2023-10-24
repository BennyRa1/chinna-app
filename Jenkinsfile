pipeline{
    agent any
      
    tools{
        maven 'maven3.5'
    }
	stages{
        stage("Git Checkout"){
            steps{
                git url:'https://github.com/BennyRa1/chinna-app.git',branch:'main'

            }
        }
        stage('Build maven'){
            steps{
                sh 'mvn clean package'
            }
        }
		stage('Generate sonarqube-analysis'){
            steps{
                withSonarQubeEnv(installationName: 'Sonar', credentialsId: 'Jenkins-sonar'){
                    sh 'mvn sonar:sonar'
                }
            }
        }
		stage('Deploy to Nexus') {
            steps {
                // Deploy the Maven artifact to the Nexus repository
                sh 'mvn deploy -DaltDeploymentRepository=chinna-app::default::http://54.146.76.166:8081//repository/chinna-app/'
            }
        }
      
        stage('Deploy war file to Tomact'){
            steps{
                sshagent(['tomcat-credentials']) {
                    sh """
                    scp -o StrictHostKeyChecking=no target/*.war ubuntu@54.174.95.56:/opt/tomcat-9/webapps
                    scp -o StrictHostKeyChecking=no ubuntu@52.203.145.155:/opt/tomcat-9/bin/shutdown.sh
                    scp -o StrictHostKeyChecking=no ubuntu@52.203.145.155:/opt/tomcat-9/bin/startup.sh
					"""
                }
            }
        }
		
	}
}	
