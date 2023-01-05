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
  stage("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                    // true = set pipeline to UNSTABLE, false = don't
                    waitForQualityGate abortPipeline: true
                }
            }
        }
  }
  
 
