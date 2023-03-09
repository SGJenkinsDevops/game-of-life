pipeline {
    agent { label 'JDK_8' }
    triggers { pollSCM ('* 7 * * 0') }
    parameters {
        choice(name: 'MAVEN_GOAL', choices: ['package', 'install', 'clean'], description: 'Maven Goal')
    }
    stages {
        stage('Version Control System') {
            steps {
                git url: 'https://github.com/SGJenkinsDevops/game-of-life.git',
                    branch: 'uat'
            }
        }
        stage('Package Creation') {
            tools {
                jdk 'JDK_8_UBUNTU'
            }
            steps {
                sh "mvn ${params.MAVEN_GOAL}"
            }
        }
        stage('Post Build') {
            steps {
                archiveArtifacts artifacts: '**/target/gameoflife-build-1.0-SNAPSHOT.jar',
                                 onlyIfSuccessful: true
                junit testResults: '**/surefire-reports/TEST-*.xml'
            }
        }
    }
}