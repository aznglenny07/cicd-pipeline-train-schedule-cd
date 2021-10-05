Skip to content
Search or jump to…
Pull requests
Issues
Marketplace
Explore
 
@aznglenny07 
aznglenny07
/
cicd-pipeline-train-schedule-cd
Public
forked from linuxacademy/cicd-pipeline-train-schedule-cd
0
03.4k
Code
Pull requests
Actions
Projects
Wiki
Security
Insights
Settings
cicd-pipeline-train-schedule-cd/Jenkinsfile
@aznglenny07
aznglenny07 Update Jenkinsfile
Latest commit 723db46 10 minutes ago
 History
 2 contributors
@whboyd@aznglenny07
74 lines (74 sloc)  3.25 KB
   
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('DeployToStaging') {
            when {
                branch 'master'
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                    sshPublisher(
                        failOnError: true,
                        continueOnError: false,
                        publishers: [
                            sshPublisherDesc(
                                configName: 'staging',
                                sshCredentials: [
                                    username: "$USERNAME",
                                    encryptedPassphrase: "$USERPASS"
                                ], 
                                transfers: [
                                    sshTransfer(
                                        sourceFiles: 'dist/trainSchedule.zip',
                                        removePrefix: 'dist/',
                                        remoteDirectory: '/tmp',
                                        execCommand: 'sudo /usr/bin/systemctl stop train-schedule && rm -rf /opt/train-schedule/* && unzip /tmp/trainSchedule.zip -d /opt/train-schedule && sudo /usr/bin/systemctl start train-schedule'
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
        }
    }
}
