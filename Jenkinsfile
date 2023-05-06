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
                    def location = "$WORKSPACE/backend/"
                    def imageName = incrementVersion(location)
                    echo "Image Name: ${imageName}"
                    def dockerimage= "anssaeed/my-repo:backend-$imageName"
                    builddockerImage (dockerimage, location)
                    dockerLogin()
                    dockerPush (imageName)
                }
            }
        }

        stage('build frontend'){
            steps{
                script{
                    echo 'building frontend docker image and pushing to dockerhub'
                    def location = "$WORKSPACE/frontend/"
                    def imageName = incrementVersion(location)
                    echo "Image Name: ${imageName}"
                    def dockerimage= "anssaeed/my-repo:frontend-$imageName"
                    builddockerImage (dockerimage, location)
                    dockerPush (imageName)
                }
            }
        }
        stage('build user-portal-frontend'){
            steps{
                script{
                    echo 'building user-portal-frontend docker image and pushing to dockerhub'
                    def location = "$WORKSPACE/user-portal-frontend/"
                    def imageName = incrementVersion(location)
                    echo "Image Name: ${imageName}"
                    def dockerimage= "anssaeed/my-repo:user-portal-frontend-$imageName"
                    builddockerImage (dockerimage, location)
                    dockerPush (imageName)
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
        stage('Version Commit') {
            steps {         
                script{
                    commitVersion()
                }
        }
     }
    }

}
