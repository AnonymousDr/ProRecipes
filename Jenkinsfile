pipeline {
    agent any
    tools{
        maven 'maven 3'
        jdk 'java 8'
    }
    stages {
        stage ('Initialize') {
            steps {
                sh '''
                git branch
                echo "PATH = ${PATH}"
                echo "M2_HOME = ${M2_HOME}"
                find ~/.m2/repository -name _maven.repositories -exec rm -v {} +;
                '''
            }
        }

        stage ('Build') {
            steps {
                sh '''
                mvn install
                '''
            }
        }
      stage('Artifact') {
        steps {
          archiveArtifacts 'target/*.jar'
          def pom = readMavenPom file: 'pom.xml'
          echo "MAVEN_RELEASE = ${pom.version}"
          sh '''
            ~/workingDir/scripts/prorecipes_gitrelease.sh
           '''
        }
      }
    }
}
