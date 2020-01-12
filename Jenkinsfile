pipeline {
   agent any

   tools {
      // Install the Maven version configured as "M3" and add it to the path.
      maven "M3"
   }

   stages {
      stage('Build') {
         steps {
            // Get some code from a GitHub repository
            git 'https://github.com/kiransre/WebGoat.git'

            // Run Maven on a Unix agent.
            sh "mvn -Dmaven.test.failure.ignore=true clean package"
         
            // To run Maven on a Windows agent, use
            // bat "mvn -Dmaven.test.failure.ignore=true clean package"
         }
      }
      stage('Scans: Master') {
         when { branch 'master' }
         step {
            parallel(
               SonarQube: {
                  sh "mvn sonar:sonar"
                  echo "Getting the analysis results .. "
                  sh "/usr/local/bin/python3 /Users/kiranmasani/parse_analysis.py --path $WORKSPACE/target/sonar/report-task.txt"
               },
               NexusLifeCycle: {
                  sh "echo 'hello world'"
               }
            )
         }
      }

      stage('Scans: Dev') {
         when { not { branch 'master' } }
         steps {
            sh "mvn sonar:sonar"
            echo "Getting the analysis results .. "
            sh "/usr/local/bin/python3 /Users/kiranmasani/parse_analysis.py --path $WORKSPACE/target/sonar/report-task.txt"
         }
      }

      stage('Nexus Repo') {
         steps {
            sh "echo 'hello world'"
         }
      }

      stage('Docker Build') {
         steps {
            sh "echo 'hello world'"
         }
      }

      stage('Deploy: DEV') {
         steps {
            sh "echo 'hello world'"
         }
      }      

   }
}