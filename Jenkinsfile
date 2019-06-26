node {
  git 'https://github.com/manrodri/WebAppUseCase.git'
  if(action == 'Deploy') {
    stage('init') {
       sh  "cd terraform"
       sh 'terraform init'
    }
    stage('plan') {
      env.NODE_ENV = "commit stage"
      print "Environment will be : ${env.NODE_ENV}"
      sh label: 'terraform plan', script: "terraform plan -out=tfplan -input=false"
      script {
          timeout(time: 10, unit: 'MINUTES') {
              input(id: "Deploy Gate", message: "Deploy environment?", ok: 'Deploy')
          }
      }
    }
    stage('apply') {
        sh label: 'terraform apply', script: "terraform apply -lock=false -input=false tfplan"
    }
  }

  if(action == 'Destroy') {
    stage('plan_destroy') {
      sh label: 'terraform plan destroy', script: "terraform plan -destroy -out=tfdestroyplan -input=false"
    }
    stage('destroy') {
      script {
          timeout(time: 10, unit: 'MINUTES') {
              input(id: "Destroy Gate", message: "Destroy environment?", ok: 'Destroy')
          }
      }
      sh label: 'Destroy environment', script: "terraform apply -lock=false -input=false tfdestroyplan"
    }
  }
}
