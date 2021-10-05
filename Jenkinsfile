def workspace ='netcore-unit-testing'
def solutionName='web-api'

def pathToProject='web-api\\web-api'

def pathToUnitTestProject='web-api-test\\web-api-test'

def publishConfiguration='debug'
def framework='netcoreapp5.0'

def zipFolderName='web-api'


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
                echo 'Deploying in Dev....'
            }
        }
    }
}
