node {
  stage('SCM') {
    checkout scm
  }
  stage('SonarQube Analysis') {
    def msbuildHome = tool 'Default MSBuild'
    def scannerHome = tool 'SonarScanner for MSBuild'
    withSonarQubeEnv() {
      bat "\"${scannerHome}\\SonarScanner.MSBuild.exe\" begin /k:\"Console_Fwk_App\""
      bat "\"${msbuildHome}\\MSBuild.exe\" /t:Rebuild"
      bat "\"${scannerHome}\\SonarScanner.MSBuild.exe\" end"
	  
	}
	}
  stage('Sonar Scan Verification'){
	def scanResult = sonarqube.getResult()
	if (scanResult == "SUCCESS"){
		def analysisProperties = sonarScanner.getAnalysisProperties()
		def qualityGateStatus = analysisProperties['sonar.qualitygate.status']
		if (qualityGateStatus == "OK"){
			echo "Accepted the Scan"
		}
  }
  }
  }
  
