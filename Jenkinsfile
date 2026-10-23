   pipeline {
     agent any
     stages {
       stage('Deploy') {
         steps {
           sh 'docker build -t todo .'
           sh 'docker rm -f todo-app || true'
           sh 'docker run -d --name todo-app -p 8001:8000 todo'
         }
       }
     }
   }
