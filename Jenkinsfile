def getGitBranchName() {
  return scm.branches[0].name
}

def branchName
def targetBranch

pipeline {
  agent any

  parameters {
    string(name: 'BRANCH_NAME', defaultValue: "${scm.branches[0].name}", description: 'Git branch name')
    string(name: 'CHANGE_ID', defaultValue: '', description: 'Git change ID for merge requests')
    string(name: 'CHANGE_TARGET', defaultValue: '', description: 'Git change ID for the target merge requests')
  }




  stages {
  stage('branch name') {
      steps {
        script {
          branchName = params.BRANCH_NAME
          echo "Current branch name: ${branchName}"
        }
      }
    }

    stage('target branch') {
      steps {
        script {
          targetBranch = branchName
          echo "Target branch name: ${targetBranch}"
        }
      }
    }
    
    stage('Github') {
      steps {
        git branch: branchName,
          url: 'https://github.com/NadaYh/ProjetDevopsNada.git',
          credentialsId: 'githubId'
      }
    }
        stage('Install Node.js Packages') {
            steps {
               
                    sh 'npm install'
                }
            
        }
        stage('Build Angular Application') {
            steps {
               
                      sh 'ng build'
             
            }
       }}}
