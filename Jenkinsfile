pipeline {
    agent any 

    stages {
      stage('Pre-Checks') {
            steps {
                echo 'Pre-Checking....'
                sh 'ansible-playbook pre_checks.yml -i $HOSTS'
            }
      }
      stage('Run Playbooks') {
        steps {
            echo 'Deploying....'
            sh 'ansible-playbook deploy_core.yml -i $HOSTS'
            sh 'ansible-playbook deploy_access.yml -i $HOSTS'
        }
      }
      stage('TEST') {
        steps {
            echo 'TEST....'
            sh 'ansible-playbook run_tests.yml -i $HOSTS'
        }
      }
    }
    post{
        always{
            echo "${currentBuild.fullDisplayName}"
            echo "${currentBuild.result}"
            echo 'reporting...."
        }
        success {
            echo 'succeed'
            sh 'test.py '${env.BRANCH_NAME}' '${currentBuild.fullDisplayName}'
                '${BUILD_URL}/logTesst/progressiveTest?start=0' 'SUCCESSFUL' '${BUILD_NUMBER}'"
        }
        failed {
            echo 'failed'
            sh 'test.py '${env.BRANCH_NAME}' '${currentBuild.fullDisplayName}'
                '${BUILD_URL}/logTesst/progressiveTest?start=0' 'FAILED' '${BUILD_NUMBER}'"
