node(){	
	def mvnHome = tool name: 'maven', type: 'maven'
	def sonarHome = tool name: 'Scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
	def repoUrl = "https://github.com/anujdevopslearn/SonarQubeCoverageJava.git"
	boolean enableScan = true
	def day = ""
	
	try {
		stage('Code Checkout'){
			checkout changelog: false, poll: false, scm: [$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'GitCreds', url: repoUrl]]]
		}
		
		stage('Build Automation & Test Cases Execution'){
			sh """
				ls -lart
				${mvnHome}/bin/mvn clean install
			"""
			day = getDayOfWeek()
		}
		
		if(enableScan && day.toString() == "4" ){
			stage('Source Code Analysis'){
				withSonarQubeEnv(credentialsId: 'SonarQubeCreds') {
					sh "${sonarHome}/bin/sonar-scanner"
				}
			}
		}
		
		stage('Deployment'){
			
		}
	} catch (e){
		echo "Jenkins Build Got Failure"
		throw e
	}
	finally {
		emailext body: 'vfbgf', subject: 'fvvf', to: 'anuj_sharma401@yahoo.com'
	}
}

def getDayOfWeek(){
	def now = new Date()
	echo now.format("yyMMdd.HHmm", TimeZone.getTimeZone('UTC'))
	
	Calendar calendar = Calendar.getInstance();
	day = now[calendar.DAY_OF_WEEK]
	return day.toString()
}
