pipeline {
    agent {label 'QA-Node'} 
    triggers { pollSCM ('* * * * *') } 
    parameters {
        choice(name: 'MAVEN_GOAL', choices: ['package', 'install', 'clean'], description: 'Maven Goal')
    }
    stages {
        stage('vcs') {
            steps { 
                git url: 'git@github.com:Giridevops-Git/spc-dsl-multi-brc.git' ,
                    branch: 'dev'

            }
        }
        stage('package') {
            steps { 
                sh "mvn ${params.MAVEN_GOAL}"

            }

        }
        stage('sonar analysis') {
            steps {
                // performing sonarqube analysis with "withSonarQubeENV(<Name of Server configured in Jenkins>)"
                withSonarQubeEnv('SONAR_CLOUD') {
                    sh 'mvn clean package sonar:sonar -Dsonar.organization=springpetclinic1'
                }
            }
        } 
        stage('post build') {
            steps {
                archiveArtifacts artifacts: '**/target/spring-petclinic-3.0.0-SNAPSHOT.jar', onlyIfSuccessful: true 
                junit testResults: '**/surefire-reports/TEST-*.xml'
            }
        }
    }
}