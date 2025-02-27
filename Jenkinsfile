pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven"
    }

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/7Akshu77/empappwar-a.git']])

                // Run Maven on a Unix agent.
                bat "mvn -Dmaven.test.failure.ignore=true clean package"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/empappwar.war'
                }
            }
        }
           stage ('Deploy to tomcat server') {
             steps{
               deploy adapters: [tomcat9(credentialsId: '4ae2d6d6-758d-4994-8835-64b98d029929', path: '', url: 'http://localhost:8080/')], contextPath: '/empapp', war: 'target/empappwar.war'

             }
         }

    }
}
