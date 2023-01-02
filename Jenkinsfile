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
	  def output = sh(returnStdout: true, script: 'SonarScanner.MSBuild.exe showdetails | grep "Quality gate is:"')
	  def result = output.trim().split(":")[1]
	  if (result == 'OK'){
		echo "SonarQube scan passed Praveen Thuyamani"
	  }else{
		error "SonarQube scan failed"
	}
  }
}
