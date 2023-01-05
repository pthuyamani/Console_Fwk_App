pipeline {
    agent any
	environment {          
     //BUILD NUMBER CHANGES
     def msbuildHome    = tool 'Default MSBuild'
     def scannerHome    = tool 'SonarScanner for MSBuild'
  }
    stages {
        stage('SCM') {
            steps {
                checkout scm
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube') {
					bat "\"${scannerHome}\\SonarScanner.MSBuild.exe\" begin /k:\"Console_Fwk_App\""
					bat "\"${msbuildHome}\\MSBuild.exe\" /t:Rebuild"
					bat "\"${scannerHome}\\SonarScanner.MSBuild.exe\" end"
	  
				}			
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
}
