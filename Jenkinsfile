pipeline {
  agent any

  stages {
    stage('Maven Package') {
      steps {
        sh 'mvn clean package'
      }
    }

    stage('Upload jar to Nexus') {
      steps {
        nexusArtifactUploader artifacts: [[artifactId: 'Jenkins-test', classifier: '', file: 'target/Jenkinstest-0.0.1-SNAPSHOT.jar', type: 'jar']],
                 credentialsId: 'c9b3d9ca-d42d-4688-9f04-5d63007b1332',
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
        emailext body: "Dette er en mail fra Jenkins pipeline\nJenkins sier:  Jobb: ${env.JOB_NAME}\nByggnummer:  ${env.BUILD_NUMBER} gikk bra!", subject: 'Jenkins-test - Ny versjon bygget!', to: 'terje@itfakultetet.no'
        mattermostSend channel: '@itfakultetet, jenkins,town-square', endpoint: 'http://mattermost.itfakultetet.no:8065/hooks/1t53s7bk4tdouywsm1bfww99na', message: "### Bare hyggelig! \n- Jenkins sier:  \nJob:  ${env.JOB_NAME}   \nByggnummer:  ${env.BUILD_NUMBER}  :tada:", text: '### Ny versjon av Jenkins-test bygget  :white_check_mark:'
      }

      failure {
        emailext body: "Dette er en mail fra Jenkins pipeline\n Jenkins says:  Job Name: ${env.JOB_NAME}   \nBuild Number:  ${env.BUILD_NUMBER} mislyktes!", subject: 'Bygging av Jenkins-test feilet', to: 'terje@itfakultetet.no'
       mattermostSend channel: '@itfakultetet, jenkins,town-square', endpoint: 'http://mattermost.itfakultetet.no:8065/hooks/1t53s7bk4tdouywsm1bfww99na', message: "### OOOps! \n- Jenkins sier:  \nJob:  ${env.JOB_NAME}   \nByggnummer:  ${env.BUILD_NUMBER} :x:", text: '### Ny versjon av Jenkins-test feilet  :x:'
      }

      always {
        archiveArtifacts artifacts: "jenkinstest-0.0.${env.BUILD_NUMBER}.tgz", fingerprint: true
      }
    }
}
