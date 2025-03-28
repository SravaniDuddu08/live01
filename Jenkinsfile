pipeline {
    agent {
        label 'pro'
    }

    stages {
        stage('git') {
            steps {
                git branch: 'main', url: 'https://github.com/SravaniDuddu08/live01.git'
            }
        }
        stage('maven') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('sonarqube') {
            steps {
                withSonarQubeEnv(installationName: 'sonarqube', credentialsId: 'sonarqube2') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
        stage('nexus') {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'vamsi', classifier: '', file: 'target/live.war', type: 'war']], credentialsId: 'nexus5', groupId: 'vamsi.maven.com', nexusUrl: '65.2.34.51:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '0.0.1-SNAPSHOT'
            }
        }
         stage('tomcat') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://13.203.203.74:8085/')], contextPath: 'live', war: 'target/live.war'
            }
        }
    }
}
