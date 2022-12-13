pipeline {
    agent any
    parameters {
        choice(name: 'GitHub Branch', choices:['main','1-branch'], description: 'to refresh the list, go to configure, disable "this build has parameters", launch build (without parameters)to reload the list and stop it, then launch it again (with parameters)')
    }
    stages {
       stage("Checkout") {
            steps {
                    checkout([$class: 'GitSCM',
                        branches: [
                            [name: '${GitHub Branch}']
                        ],
                        doGenerateSubmoduleConfigurations: false,
                        extensions: [[$class: 'LocalBranch', localBranch: "**"]],
                        submoduleCfg: [],
                        userRemoteConfigs: [
                            [credentialsId: '6fa4c8a9-14a9-44e0-8630-b540766d146d', url: 'https://github.com/manojsubramaniam/test04.git']
                        ]
                    ])
                }
            }
        stage("Branch To Build") {
            steps {
                    script {
                        def gitBranches = sh(returnStdout: true, script: 'git rev-parse --abbrev-ref --all | sed s:origin/:: | sort -u')
                        env.BRANCH_TO_BUILD = input message: 'Please select a branch', ok: 'Continue',
                            parameters: [choice(name: 'BRANCH_TO_BUILD', choices: gitBranches, description: 'Select the branch to build?')]
                    }
                    git branch: "${env.BRANCH_TO_BUILD}", credentialsId: '6fa4c8a9-14a9-44e0-8630-b540766d146d', url: 'https://github.com/manojsubramaniam/test04.git'
                }
            }
          }
    post {
      always {
        echo 'Cleanup'
        cleanWs()
        }
    }
  }
