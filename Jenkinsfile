pipeline {
  agent any

  stages {
    stage('Maven clean package') {
      steps {
        sh 'mvn clean package'
      }
    }

    stage('Upload jar to Nexus') {
      steps {
        nexusArtifactUploader artifacts: [[artifactId: 'Jenkins-test', classifier: '', file: 'target/Jenkinstest-0.0.1-SNAPSHOT.jar', type: 'jar']],
                 credentialsId: 'ba94238c-4b4c-440c-9e78-62de2ee9d890',
                 groupId: 'no.itfakultetet',
                 nexusUrl: 'itfakultetet.no:8081',
                 nexusVersion: 'nexus3',
                 protocol: 'http',
                 repository: 'Jenkinskurs',
                 version: '0.0.1-SNAPSHOT'
      }
    }
  }
    post {
      success {
        emailext body: "Dette er en mail fra Jenkins pipeline<br><b>Jenkins sier:</b> <br> Jobb: ${env.JOB_NAME}<br><b>Byggnummer:</b>  ${env.BUILD_NUMBER} gikk bra!", subject: 'Jenkins-test - Ny versjon bygget!', to: 'terje@itfakultetet.no'
        mattermostSend channel: '@itfakultetet, jenkins,town-square', endpoint: 'http://mattermost.itfakultetet.no:8065/hooks/1t53s7bk4tdouywsm1bfww99na', message: "### Bare hyggelig! \n- Jenkins sier:  \nJob:  ${env.JOB_NAME}   \nByggnummer:  ${env.BUILD_NUMBER}  :tada:", text: '### Ny versjon av Jenkins-test bygget  :white_check_mark:'
      }

      failure {
       emailext body: "Dette er en mail fra Jenkins pipeline<br><b>Jenkins sier</b>: <br><b>Jobb</b>: ${env.JOB_NAME}<br><b>Byggnummer:</b>  ${env.BUILD_NUMBER} mislyktes!", subject: 'Bygging av Jenkins-test feilet', to: 'terje@itfakultetet.no'
       mattermostSend channel: '@itfakultetet, jenkins,town-square', endpoint: 'http://mattermost.itfakultetet.no:8065/hooks/1t53s7bk4tdouywsm1bfww99na', message: "### OOOps! \n- Jenkins sier:  \nJob:  ${env.JOB_NAME}   \nByggnummer:  ${env.BUILD_NUMBER} :x:", text: '### Ny versjon av Jenkins-test feilet  :x:'
      }

      always {
        archiveArtifacts artifacts: "target/Jenkinstest-0.0.1-SNAPSHOT.jar", fingerprint: true
      }
    }
}
