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
        stage('Backend - Test'){
            steps {
                dir('10-net9-remix-pg-env/Backend') {
                    echo 'Running tests...'
                    sh 'dotnet test --no-build --verbosity normal'
                }
            }
        }
        stage('Backend - Code Coverage'){
            steps {
                dir('10-net9-remix-pg-env/Backend') {
                    echo 'Running code coverage...'
                    sh 'dotnet test --collect:"XPlat Code Coverage" --no-build --verbosity normal'
                }
            }
        }
        stage('Backend - Build'){
            steps {
                dir('10-net9-remix-pg-env/Backend') {
                    echo 'Building the project...'
                    sh 'dotnet build --configuration Release --no-restore'
                }
            }
        }
        stage('Backend - Publish'){
            steps {
                dir('10-net9-remix-pg-env/Backend') {
                    echo 'Publishing the project...'
                    sh 'dotnet publish --configuration Release --no-build -o ./publish'
                }
            }
        }
        stage('Frontend - Install'){
            steps {
                dir('10-net9-remix-pg-env/Frontend') {
                    echo 'Installing dependencies...'
                    sh 'npm install'
                }
            }
        }
        stage('Frontend - Test'){
            steps {
                dir('10-net9-remix-pg-env/Frontend') {
                    echo 'Running tests...'
                    sh 'npm test'
                }
            }
        }
        stage('Frontend - Build'){
            steps {
                dir('10-net9-remix-pg-env/Frontend') {
                    echo 'Building the project...'
                    sh 'npm run build'
                }
            }
        }
    }

    post {
        always {
            echo 'This will always run after the stages.'
        }
    }
}
