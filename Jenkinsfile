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
  stage ('Sonar Scan Verification'){
	def sonarScannerOutpu = sh(script: "sonar-scanner",returnStdout:true)
	if (sonarScannerOutput.exitValue() == 0){
		println "SonarQube scan successfully accepted"
	} else {
		println "SonarQube scan failed and rejected"
  }
  }
  }
