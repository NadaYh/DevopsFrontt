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
          url: 'https://github.com/NadaYh/DevopsFrontt.git',
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
       }
        stage('Publish to Docker Hub') {
            steps {
                script {
                    // Push the Angular app's Docker image to Docker Hub
                    withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: "${regsryCredential}", usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']]) {
                        docker.withRegistry("${registry}", "${regsryCredential}") {
                            sh "docker build -t ${registry}/${dockerImage} ."
                            sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                            sh "docker push ${registry}/${dockerImage}"
                        }
                    }
                }
            }
        }
  }}
