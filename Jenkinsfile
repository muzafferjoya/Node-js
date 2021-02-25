pipeline {
  agent any  
  stages {
    stage('Install Packages') {
      steps {
        sh 'npm install'
      }
    }
    stage('Test and Build') {
      parallel {
        stage('Run Tests') {
          steps {
            sh 'npm run test'
          }
        }
        stage('Create Build Artifacts') {
          steps {
            sh 'npm run build'
          }
        }
      }
    }
    stage('Deployment') {
      parallel {
        stage('Staging') {
          when {
            branch 'master'
          }
          steps {
            withAWS(region:'us-east-1',credentials:'4642b1e0-e10c-47f5-92c4-410b3aeceaef') {
              
              s3Upload(bucket: 'eroam-frontend', workingDir:'build', includePathPattern:'**/*');
            }
            mail(subject: 'Staging Build', body: 'New Deployment to Staging', to: 'muzaffar.khan@eroam.com')
          }
        }
        
      }
    }
  }
}
