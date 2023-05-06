library identifier: 'jenkins-shared-library@master', retriever: modernSCM(
    [
        $class: 'GitSCMSource',
        remote: 'https://github.com/Ans-Saeed/jenkins-shared-library',
        credentialsId: 'github-credentials'
    ]
)
def gv
pipeline{
    agent any
    stages {

        stage('init'){
            steps{
                script{
                    gv = load "script.groovy"
                }
            }
        }
        stage('build server'){
            steps{
                script{
                    echo 'building the backend server and pushing to dockerhub...'
                    builddockerImage ('anssaeed/my-repo:backend1.0', '/var/jenkins_home/workspace/online-exam-portal-pipeline/backend/')
                    dockerLogin()
                    dockerPush'anssaeed/my-repo:server1.0'
                }
            }
        }

        stage('build frontend'){
            steps{
                script{
                    echo 'building frontend docker image and pushing to dockerhub'
                     builddockerImage ('anssaeed/my-repo:frontend1.0', '/var/jenkins_home/workspace/online-exam-portal-pipeline/frontend/')
                    // dockerLogin()
                    dockerPush'anssaeed/my-repo:frontend1.0'
                }
            }
        }
        stage('build user-portal-frontend'){
            steps{
                script{
                    echo 'building user-portal-frontend docker image and pushing to dockerhub'
                    builddockerImage ('anssaeed/my-repo:userportal1.0', '/var/jenkins_home/workspace/online-exam-portal-pipeline/user-portal-frontemd/')
                    // dockerLogin()
                    dockerPush'anssaeed/my-repo:userportal1.0'
                }
            }
        }
        stage('deploy'){
            steps{
                script{
                    echo 'deploying the application'
                }
            }
        }
    }

}
