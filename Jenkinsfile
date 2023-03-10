pipeline {
    agent { label 'JDK_8' }
    triggers { pollSCM ('* * * * *') }
    parameters {
        choice(name: 'MAVEN_GOAL', choices: ['package', 'install', 'clean'], description: 'Maven Goal')
    }
    stages {
        stage('Version Control System') {
            steps {
                git url: 'https://github.com/SGJenkinsDevops/game-of-life.git',
                    branch: 'stashUnstash'
            }
        }
        stage('Package Creation') {
            tools {
                jdk 'JDK_8_UBUNTU'
            }
            steps {
                sh "mvn clean"
                sh "mvn ${params.MAVEN_GOAL}"
            }
        }
        stage('Post Build') {
            steps {
                archiveArtifacts artifacts: '**/target/gameoflife-build-1.0-SNAPSHOT.jar',
                                 onlyIfSuccessful: true
                junit testResults: '**/surefire-reports/TEST-*.xml'
                stash name: 'spc-jar',
                    include: '**/target/gameoflife-build-1.0-SNAPSHOT.jar'
            }
        }
        stage('Ansible') {
            agent { label 'JDK_17' }
            steps {
                unstash name: 'spc-jar'
            }
        }
    }
}