pipeline{
    agent any
	
	environment {
        // Define environment variables
        TOMCAT_SERVER = '100.25.221.73'
        TOMCAT_USER = 'ubuntu'
        TOMCAT_KEY_CREDENTIAL_ID = 'demo-tom'
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
                    sshagent(credentials: [TOMCAT_KEY_CREDENTIAL_ID]) {
                        sh "scp target/your-web-app.war ${TOMCAT_USER}@${TOMCAT_SERVER}:/opt/tomcat-9/webapps/"
                    }
                }
            }
        }
}	
}
