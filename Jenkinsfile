pipeline {
 agent any
 tools {
  maven 'Maven-3.5.2'
 }
 stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }

        stage('SCM Checkout')  {
            steps {
             checkout scm
            }
        }

        stage('Compile') {
            steps {
                sh label: '', script: 'mvn clean compile'
            }
        }

       stage('Application_Unit_Test'){
            steps {
                sh label: '', script: 'mvn compiler:testCompile -Dfilename=testng-unit.xml surefire:test'
            }
            post{
                always{
                    step([$class: 'Publisher'])
                }
            }
        }
        stage('Code Coverage'){
            steps {

                //sh 'mvn cobertura:cobertura -Dcobertura.report.format=xml -Dsurefire.suiteXmlFiles=testng-unit.xml '
                  sh ' mvn clean install cobertura:cobertura  -Dcobertura.report.format=xml -Dsurefire.suiteXmlFiles=testng-unit.xml'
                //  sh 'cat target/site/cobertura/coverage.xml'
            }

        }
        stage('SonarQube analysis') {
        steps {
         sh 'ls -lrt'
        withSonarQubeEnv('Sonarqube') { // If you have configured more than one global server connection, you can specify its name
        sh "mvn sonar:sonar"
        }
      }

  }

  stage('SonarQube analysis') {
  steps {
    script{
    timeout(time: 3, unit: 'MINUTES') {
      def qg = waitForQualityGate()
      if (qg.status != 'OK'){
        error "pipeline aborted due to quality gate failure: ${qg.status}"
      }
    }
    }

  }
}

        stage('Package') {
            steps {
                sh label: '', script: 'mvn clean package -DskipTests'
            }
        }


       /*stage('deploy'){
         steps {

       sshagent(['deploykeys']) {
         sh 'whoami'
        sh 'scp -o StrictHostKeyChecking=no target/*.war ec2-user@52.55.54.236:/opt/tomcat/webapps'
    }
         }
     }*/

 }
}
