def workspace ='netcore-unit-testing'
def solutionName='web-api'

def pathToProject='web-api\\web-api'

def pathToUnitTestProject='web-api-tests\\web-api-tests'

def publishConfiguration='debug'
def framework='netcoreapp5.0'

def zipFolderName='web-api'

def iisApplicationName='testpipeline'


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
                bat "dotnet build ${pathToProject}.csproj /T:Publish /p:configuration=${publishConfiguration} /p:framework=${framework} /p:WebPublishMethod=Package /p:PackageAsSingleFile=true /p:DesktopBuildPackageLocation=\"bin\\debug\\webpackage\\${zipFolderName}.zip\""
     
            }
        }
        
         
        stage('Deploy(Dev)') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'IIS_JENKINS', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    bat "\"C:\\Program Files (x86)\\IIS\\Microsoft Web Deploy V3\\msdeploy.exe\" -verb=sync -source:package=\"web-api\\bin\\debug\\webpackage\\${zipFolderName}.zip\" -dest:auto,computerName=\"https://13.234.53.206:8172/msdeploy.axd?site=Default Website\",userName=${USERNAME},password=${PASSWORD},authType=basic -setParam:\"IIS Web Application Name\"=\"Default Website/${iisApplicationName}\" -allowUntrusted=true -enableRule:DoNotDeleteRule -enableRule:AppOffline"
                }
	
                echo 'Deploying in Dev....'
            }
        }
    }
}
