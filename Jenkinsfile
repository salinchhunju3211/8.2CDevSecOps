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
        // Continue even if tests fail or need auth (e.g., snyk test)
        bat 'cmd /c npm test || exit /b 0'
      }
    }

    stage('Generate Coverage Report') {
      steps {
        // Continue even if the project has no "coverage" script
        bat 'cmd /c npm run coverage || exit /b 0'
      }
    }

    stage('NPM Audit (Security Scan)') {
      steps {
        // Show vulnerabilities but don't fail the build
        bat 'cmd /c npm audit || exit /b 0'
      }
    }

    stage('SonarCloud Analysis') {
      steps {
        // Requires a Secret Text credential in Jenkins with ID = SONAR_TOKEN
        withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
          // Uses pre-installed SonarScanner CLI on PATH
          bat 'sonar-scanner -D"sonar.login=%SONAR_TOKEN%"'
        }
      }
    }
  }
}
