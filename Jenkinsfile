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

        stage('version bump'){
            steps{
                script{
                    def imageName = incrementVersion('location_argument')
                    echo "Image Name: ${imageName}"
                }
            }

        stage('build server'){
            steps{
                script{
                    echo 'building the backend server and pushing to dockerhub...'
                    def location = "/var/jenkins_home/workspace/e-online-exam-portal_jenkins-job/backend/"
                    def imageName = incrementVersion("$location")
                    echo "Image Name: ${imageName}"
                    builddockerImage ("$imageName", "$location")
                    dockerLogin()
                    dockerPush "$imageName"
                }
            }
        }

        stage('build frontend'){
            steps{
                script{
                    echo 'building frontend docker image and pushing to dockerhub'
                    def location = "/var/jenkins_home/workspace/e-online-exam-portal_jenkins-job/frontend/"
                    def imageName = incrementVersion("$location")
                    echo "Image Name: ${imageName}"
                    builddockerImage ("$imageName", "$location")
                    dockerPush "$imageName"
                }
            }
        }
        stage('build user-portal-frontend'){
            steps{
                script{
                    echo 'building user-portal-frontend docker image and pushing to dockerhub'
                    def location = "/var/jenkins_home/workspace/e-online-exam-portal_jenkins-job/user-portal-frontend/"
                    def imageName = incrementVersion("$location")
                    echo "Image Name: ${imageName}"
                    builddockerImage ("$imageName", "$location")
                    dockerPush "$imageName"
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
}