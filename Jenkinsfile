pipeline {
    agent any

    stages {
        stage("Checkout code from GitHub") {
            steps {
                git branch: 'main', url: 'https://github.com/dessisdakova/SelenuimWebDriver-Calc-practice-2'
            }
        }

        stage("Setting up .Net 6.0 SDK") {
            steps {
                bat '''
                echo Downloading .Net 6 Sdk
                curl -l -o dotnet-sdk-6.0.132-win-x86.exe https://download.visualstudio.microsoft.com/download/pr/ad59f1d1-5f19-4474-86be-2f09ab195618/5c7a64445dae84e386bb88e1f6ac09e4/dotnet-sdk-6.0.132-win-x86.exe
                echo Installing dotnet-sdk-6.0.132-win-x86.exe
                dotnet-sdk-6.0.132-win-x86.exe /quiet /norestart
                '''
            }
        }

        stage("Installing Nuget packeges") {
            steps {
                bat 'dotnet restore SeleniumBasicExercise.sln'
            }
        }

        stage("Build") {
            steps {
                bat 'dotnet build SeleniumBasicExercise.sln --configuration Release'
            }
        }

        stage("Run tests in TestProject1") {
            steps {
                bat 'dotnet test TestProject1\\TestProject1.csproj --logger "trx;LogFileName=TestResults.trx"'
            }
        }

        stage("Run tests in TestProject2") {
            steps {
                bat 'dotnet test TestProject2\\TestProject2.csproj --logger "trx;LogFileName=TestResults.trx"'
            }
        }

        stage("Run tests in TestProject3") {
            steps {
                bat 'dotnet test TestProject3\\TestProject3.csproj --logger "trx;LogFileName=TestResults.trx'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/TestResults/*.trx', allowEmptyArchive: true
            step([
                $class: 'MSTestPublisher',
                testResultsFile: '**/TestResults/*.trx'
            ])
        }
    }
}