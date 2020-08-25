pipeline {
    agent any
    tools {
        maven 'Maven 3.6.3'
    }

    stages {
        stage('verify') {
            steps {
                // Vérifier si Maven installé
                sh 'mvn -v'
            }
        }
        stage('compile') {
            steps {
                // Run Maven on a Unix agent.
                sh 'mvn clean package'
            }
        }
        stage('test') {
            steps {
                sh 'mvn test'
            }
            post {
                unstable {
                    echo 'Tests non passants'
                }
                failure {
                    slackSend channel: 'jenkins-training', color: '#FF0000', message: 'Echec de Jérémy', teamDomain: 'devinstitut', tokenCredentialId: '29e2c071-d271-40c4-bf58-736d5147f892'
                }
                success {
                    junit 'target/surefire-reports/*xml'
                    archiveArtifacts 'target/petclinic.war'
                    slackSend channel: 'jenkins-training', color: '#00FF00', message: 'Succès de Jérémy', teamDomain: 'devinstitut', tokenCredentialId: '29e2c071-d271-40c4-bf58-736d5147f892'
                }
            }
        }
    }
}
