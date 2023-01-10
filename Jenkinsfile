def imageName = 'movies-store'
def registry = 'chuhuynh.jfrog.io/default-docker-local'

pipeline{
   
    agent any
    stages{
        stage('Checkout'){
            steps{
                checkout scm
            }
        }

        // stage('Unit Tests'){
        //     steps{
        //         script {
        //             def imageTest= docker.build("${imageName}-test", "-f Dockerfile.test .")
        //             def imageVersion = commitID()
        //             sh "docker build  -t ${image_repo}:${imageName}-test -f ${dockerpath} ."
        //             imageTest.inside{
        //                 sh "python test_main.py"
        //             }
        //         }
        //     }
        // }

        stage('Build'){
            steps{
                script {
                    def imageVersion = commitID()
                    sh "docker build  -t ${registry}/${imageName}:${imageVersion} ."
                }
            }
        }

        stage('Push'){
            steps{
                script {
                    def imageVersion = commitID()

                        withCredentials([usernamePassword(credentialsId: 'jfrog-credentials-id', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        LOGIN_RESULT = sh(script: "/bin/bash -c 'echo $PASSWORD | docker login -u $USERNAME chuhuynh.jfrog.io --password-stdin'", returnStdout: true)
                        echo "${LOGIN_RESULT}"
                        sh "docker push ${registry}/${imageName}:${imageVersion}"
                        }
                    
                }
            }
        }
    }
}

def commitID() {
    sh 'git rev-parse HEAD > .git/commitID'
    def commitID = readFile('.git/commitID').trim()
    sh 'rm .git/commitID'
    return commitID
}
