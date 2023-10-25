pipeline{
    agent any

    }
	
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
                sh 'mvn deploy -DaltDeploymentRepository=chinna-app::default::http://54.146.76.166:8081/repository/chinna-app/'
            }
        }
		stage('Deploy to Tomcat') {
            steps {
                script {
                    // Deploy to Tomcat
                  sh  "scp -i /home/ec2-user/aws.pem /var/lib/jenkins/workspace/tomcat/target/hiring.war ubuntu@100.25.221.73:/home/ubuntu/"
                }
            }
        }
}	
}
