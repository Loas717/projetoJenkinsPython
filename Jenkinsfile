pipeline {
    agent any

    stages {
        stage('Preparar ambiente') {
            steps {
                bat """
                    py -m venv venv
                    call venv\\Scripts\\activate.bat
                    py -m pip install --upgrade pip
                    py -m pip install -r requirements.txt
                """
            }
        }

        stage('Executar testes') {
            steps {
                bat """
                    call venv\\Scripts\\activate.bat
                    py -m pytest --junitxml=relatorio-testes.xml
                """
            }
        }

        stage('Build') {
            steps {
                bat """
                    call venv\\Scripts\\activate.bat
                    py -m compileall app
                """
            }
        }

        stage('Iniciar aplicacao') {
            steps {
                bat """
                    taskkill /F /IM python.exe /T || exit /B 0
                    start /B py -m app.main
                """
            }
        }
    }

    post {
        always {
            junit allowEmptyResults: true, testResults: 'relatorio-testes.xml'
        }
    }
}