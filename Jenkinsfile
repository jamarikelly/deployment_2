pipeline {
  agent any
   stages {
    stage ('Build') {
      steps {
        sh '''#!/bin/bash
        python3 -m venv test3
        source test3/bin/activate
        pip install pip --upgrade
        pip install -r requirements.txt
        export FLASK_APP=application
        flask run &
        '''
     }
   }
    
     stage ('test') {
      steps {
       echo "HELLO!"
      }
     }
     stage ('Deploy') {
       steps {
         sh '/var/lib/jenkins/.local/bin/eb deploy workspace-dev'
       }
     }
  }
 }
