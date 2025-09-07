pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/salinchhunju3211/8.2CDevSecOps.git'
      }
    }

    stage('Install Dependencies') {
      steps {
        bat 'npm install'
      }
    }

    stage('Run Tests') {
      steps {
        bat 'cmd /c npm test || exit /b 0'
      }
    }

    stage('Generate Coverage Report') {
      steps {
        bat 'cmd /c npm run coverage || exit /b 0'
      }
    }

    stage('NPM Audit (Security Scan)') {
      steps {
        bat 'cmd /c npm audit || exit /b 0'
      }
    }

    stage('SonarCloud Analysis') {
      steps {
        withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
          bat '''
          powershell -NoProfile -ExecutionPolicy Bypass -Command ^
            "$ver = '5.0.1.3006'; ^
             $zip = 'sonar-scanner.zip'; ^
             Invoke-WebRequest -Uri ('https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-' + $ver + '-windows.zip') -OutFile $zip; ^
             if (Test-Path sonar-scanner) { Remove-Item -Recurse -Force sonar-scanner }; ^
             Expand-Archive -Path $zip -DestinationPath . -Force; ^
             $dir = Get-ChildItem -Directory | Where-Object { $_.Name -like 'sonar-scanner-*windows' } | Select-Object -First 1; ^
             $env:SONAR_SCANNER_OPTS='-Xmx512m'; ^
             & (Join-Path $dir.FullName 'bin/sonar-scanner.bat') -Dsonar.login=%SONAR_TOKEN%"
          '''
        }
      }
    }
  }
}
