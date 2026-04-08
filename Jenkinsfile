node {
  stage('SCM') {
    checkout scm
  }
  stage('SonarQube Analysis') {
    steps {
        script {
            def scannerHome = tool 'SonarQubeScanner'

            withSonarQubeEnv('SonarQube') {

                withVault([
                    vaultSecrets: [[
                        path: 'secret/data/tools/sonarqube/token',
                        secretValues: [
                            [envVar: 'SONAR_TOKEN', vaultKey: 'SONAR_TOKEN']
                        ]
                    ]]
                ]) {

                    sh """
                        ${scannerHome}/bin/sonar-scanner \
                          -Dsonar.projectKey=jenkins-cicd \
                          -Dsonar.sources=. \
                          -Dsonar.host.url=http://sonarqube:9000 \
                          -Dsonar.token=$SONAR_TOKEN
                    """
                }
            }
        }
    }
}
}