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
	when {
		def report = httpRequest url: 'http://localhost:9000/api/qualitygates/project_status',
								 auth: ['username':'admin','password':'admin123'],
								 customHeaders:[[name:'Content-Type', value:'application/json']],
								 requestBody: """{"projectKey": "Console_Fwk_App"}"""
		
		def analysisStatus = readJSON(text: report.content).qualityGate.status
		expression { analysisStatus == "OK"}
	}
	steps{
		echo "Accepted and Successful"
  
  }
  }
  }
  
