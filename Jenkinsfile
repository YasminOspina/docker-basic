pipeline {
    agent any
    
    tools {
        dotnetsdk 'dotnet-9.0.203'
        nodejs 'Node-20.19.2'
    }

    environment {
        DOTNET_ROOT = "${env.PATH}:${tool 'dotnet-9.0.203'}/bin"
        PATH = "${env.PATH}:${tool 'Node-20.19.2'}/bin:${env.HOME}/.dotnet/tools"
    }

    stages {
        stage('Check versions'){
            steps {
                script {
                    echo 'Node.js version:'
                    sh 'node -v'
                    echo 'NPM version:'
                    sh 'npm -v'
                    echo 'Dotnet version:'
                    sh 'dotnet --version'
                }
            }
        }
        stage('Backend - Restore'){
            steps {
                dir('10-net9-remix-pg-env/Backend') {
                    echo 'Restoring dependencies...'
                    sh 'dotnet restore'
                }
            }
        }
        stage('Backend - Static Analysis'){
            steps {
                dir('10-net9-remix-pg-env/Backend') {
                    echo 'Running static analysis...'
                    sh 'dotnet sonarscanner begin /k:"Docker-Basic" /d:sonar.host.url="http://localhost:9000" /d:sonar.login="sqa_0bd1bd3ed74003eb34c11af51176a1fd72c4e5aa"'
                    sh 'dotnet build'
                    sh 'dotnet sonarscanner end /d:sonar.login="sqa_0bd1bd3ed74003eb34c11af51176a1fd72c4e5aa"'
                }
            }
        }
    }
}
