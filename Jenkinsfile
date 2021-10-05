def workspace ='netcore-unit-testing'
def solutionName='web-api'

def pathToProject='web-api\\web-api'

def pathToUnitTestProject='web-api-tests\\web-api-tests'

def publishConfiguration='debug'
def framework='netcoreapp5.0'

def zipFolderName='web-api'

def iisApplicationName='testpipeline'



def jenkinsCredentialsId='IIS_JENKINS'
def path='web-api'
def deploymentTarget='debug'
def projName='web-api'
def deployURL='https://13.234.53.206:8172'
def iisSiteParent='Default Website'
def iisSiteName='testpipeline'


pipeline {
    agent { node { label 'WINAGENT02' } }

    stages {
        
        stage('Restore') {
            steps {
                echo 'Restore..  dotnet restore  '
                  bat "dotnet restore ${solutionName}.sln"
            }
        }
        stage('Unit Test') {
		when{
			not{
				branch 'skip-unit-test'
			}			
		}
            steps {
                echo 'Testing.. dotnet test  '
                  bat "dotnet test ${pathToUnitTestProject}.csproj"
            }
        }
        stage('Compile & Zip') {
            steps {
                echo 'Compile..  dotnet build  '
                bat "dotnet build ${pathToProject}.csproj /T:Publish /p:configuration=${deploymentTarget} /p:framework=${framework} /p:WebPublishMethod=Package /p:PackageAsSingleFile=true /p:DesktopBuildPackageLocation=\"bin\\${deploymentTarget}\\webpackage\\${projName}.zip\""
     
            }
        }
        
         
        stage('Deploy(Dev)') {
            steps { 
		    withCredentials([usernamePassword(credentialsId: "${jenkinsCredentialsId}", usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
			    bat "\"C:\\Program Files (x86)\\IIS\\Microsoft Web Deploy V3\\msdeploy.exe\" -verb=sync -source:package=\"${path}\\bin\\${deploymentTarget}\\webpackage\\${projName}.zip\" -dest:auto,computerName=\"${deployURL}/msdeploy.axd?site=${iisSiteParent}\",userName=${USERNAME},password=${PASSWORD},authType=basic -setParam:\"IIS Web Application Name\"=\"${iisSiteParent}/${iisSiteName}\" -allowUntrusted=true -enableRule:DoNotDeleteRule -enableRule:AppOffline"
		    }
		    //msdeploy(path, projName, deployURL, deploymentTarget, iisSiteParent, iisSiteName, jenkinsCredentialsId) 	
		    echo 'Deploying in Dev....'
            }
        }
    }
}
