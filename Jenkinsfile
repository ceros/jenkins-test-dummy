#!/usr/bin/env groovy

tools.createTrigger('abc123', 'github.com/ceros/enkins-test-dummy', ['release/*'])
tools.initPipeline()

pipeline {
  label 'jenkins-ecs-slave'

  stages {
    stage ('Merge release into develop') {
      steps {
        script {
        
          tools.scmMerge("https://github.com/${repository}", ${branch}, 'develop')

        }
      }
    }
    
    stage('Push to develop') {
      // This is currently the best way to push a branch from a Pipeline 
      // job. https://issues.jenkins-ci.org/browse/JENKINS-28335 is an
      // open JIRA for getting the GitPublisher Jenkins functionality
      // working with Pipeline     
      steps {
        script {
          def creds = tools.getCredentials(CerosVars.GITHUB_CREDENTIALS_ID);
          sh "git push https://${creds}@github.com/${repository} HEAD:develop"    
            
        }
      }
    }
  }
}
