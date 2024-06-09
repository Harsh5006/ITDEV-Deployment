pipeline {
    agent any
    
    triggers {
        GenericTrigger(
            genericVariables: [
                [key: 'IF_MERGED', value: '$.pull_request.merged'],
                [key: 'BRANCH', value: '$.pull_request.base.ref'],
                [key: 'DIFFURL', value: '$.pull_request.diff_url']
            ],
            causeString: 'Triggered by Webhook',
            token: '4e4245fr589fu',
            printContributedVariables: true,
            printPostContent: true,
            silentResponse: false,
            regexpFilterText: '$IF_MERGED$BRANCH',
            regexpFilterExpression: 'truemain'
        )
    }
    
    stages {
        stage('Clone Repository'){
            steps{
                script{
                    dir('repo1') {
                        checkout([$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/Harsh5006/ITDEV-Deployment.git']]])
                    }
                    dir('repo2') {
                        checkout([$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/Harsh5006/Validation-Script.git']]])
                    }
                }
            }
        }
        stage('Invoke Python Script'){
            steps{
                sh 'pip install requests pyyaml'
                sh 'python3 repo2/trigger_jenkins_job.py $DIFFURL'
            }
        }
    }
}
