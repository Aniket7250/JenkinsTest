def workspace ='netcore-unit-testing'
def solutionName='web-api'

def pathToProject='web-api\\web-api'

def pathToUnitTestProject='web-api-tests\\web-api-tests'

def publishConfiguration='debug'
def framework='netcoreapp5.0'

def zipFolderName='web-api'


pipeline

{

    agent any

    stages

    {

        stage('Setup parameters')

        {

            steps {

                script {

                    properties([

                        parameters([

                            choice(

                                choices: ['Develop', 'Release','Master'],

                                name: 'Branch'

                            ),

                          

                            text(

                                defaultValue: '''

                                Mention the features that

                                 are added / updated

                                ''',

                                 name: 'Features'

                            ),

                            string(

                                defaultValue: 'Tag and Commit Id',

                                name: 'Sprint',

                                trim: true

                            )

                        ])

                    ])

                   

                }

            }

        }

        stage ('Build Skipped') {

            when {

                expression {

                    Branch == 'Develop'

                }

            }

            steps {

                echo 'Skipped full build.'

            }

        }

    }  

}
