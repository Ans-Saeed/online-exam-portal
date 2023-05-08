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
    // environment {
    //     BACKEND_IMAGE = 'backend image'
    //     FRONTEND_IMAGE = 'frontend image'
    //     USER_FRONTEND_IMAGE = 'user frontend image'
    // }
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
                    env.BACKEND_IMAGE = "anssaeed/my-repo:backend-$imageName"
                    builddockerImage (env.BACKEND_IMAGE, location)
                    dockerLogin()
                    dockerPush (env.BACKEND_IMAGE)
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
                    env.FRONTEND_IMAGE= "anssaeed/my-repo:frontend-$imageName"
                    builddockerImage (env.FRONTEND_IMAGE, location)
                    dockerPush (env.FRONTEND_IMAGE)
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
                    env.USER_FRONTEND_IMAGE= "anssaeed/my-repo:user-portal-frontend-$imageName"
                    builddockerImage (env.USER_FRONTEND_IMAGE, location)
                    dockerPush (env.USER_FRONTEND_IMAGE)
                    }
            }
        }

        stage('deploy'){
            steps{
                script{
                    echo 'deploying the application'
                    def dockerCmd = "docker run -p 5000:5000 --network my-network --name backend -d ${env.BACKEND_IMAGE} && docker run -p 3100:3000 --network my-network --name frontend -d ${env.FRONTEND_IMAGE} && docker run -p 3200:3000 --network my-network --name user-frontend -d ${env.USER_FRONTEND_IMAGE}"
                    sshagent(['ec2user-server-ssh-key']) {
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@13.126.173.83 '${dockerCmd}'"
                    }
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
