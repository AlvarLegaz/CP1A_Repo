pipeline {
    agent any

    stages {
        stage('Get Code') {
            steps {
                // Obtener codigo del repo
                git 'https://github.com/AlvarLegaz/CP1A_Repo.git'
            }
        }
        
        stage('Build') {
            steps{
                echo 'Eyy, esto es Python. No hay que compilar nada!!!'
                echo WORKSPACE
                bat 'dir'
            }
        }
        
        stage('Tests'){
            parallel{
                
                stage('Unit') {
                    steps {
                        catchError(buildResult: 'UNSTABLE', stageResult: 'FAILURE'){
                            bat '''
                                set PYTHONPATH=.
                                pytest --junitxml=result-unit.xml test\\unit
                                '''
                        }
                    }
                }
                
                stage('Rest') {
                    steps {
                        catchError(buildResult: 'UNSTABLE', stageResult: 'FAILURE'){
                            bat '''
                                set FLASK_APP=app\\api.py
                                start flask run
                                start java -jar C:\\Users\\ALegaz\\Desktop\\CP1A_Repo_working\\Wiremock\\wiremock-standalone-3.5.3.jar --port 9090 --root-dir test\\wiremock          
                                
                                set PYTHONPATH=%WORKSPACE%
                                pytest --junitxml=result-rest.xml test/rest
                            '''
                        }
                    }
                }
            }
        }
        
        stage('Results'){
            steps {
                junit 'result*.xml'
                echo 'FINISH!!!'
            }
        }
    }
}
