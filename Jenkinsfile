pipeline {
    agent any
    triggers {
        pollSCM "* * * * *"
    }

    stages {

        // config
        stage('Build Application') {
            steps {
                echo '=== Building Petclinic Application ==='
                sh 'mvn -B -DskipTests clean package'
                sh 'exit 1'
            }
        }
        /* stage('Test Application') {
             steps {
                 echo '=== Testing Petclinic Application ==='
                 sh 'mvn test'
             }
             post {
                 always {
                     junit 'target/surefire-reports/*.xml'
                 }
             }
         }
         stage('Build Docker Image') {
             steps {
                 echo '=== Building Petclinic Docker Image ==='
                 script {
                     app = docker.build("vivek796/petclinic-jenkins-jfrog")
                 }
             }
         }
         stage('Push Docker Image') {
             steps {
                 echo '=== Pushing Petclinic Docker Image ==='
                 script {
                     GIT_COMMIT_HASH = sh(script: "git log -n 1 --pretty=format:'%H'", returnStdout: true)
                     SHORT_COMMIT = "${GIT_COMMIT_HASH[0..7]}"
                     docker.withRegistry('https://registry.hub.docker.com', 'dockerHubCredentials') {
                         app.push("$SHORT_COMMIT")
                         app.push("latest")
                     }
                 }
             }
         }
         stage('Remove local images') {
             steps {
                 echo '=== Delete the local docker images ==='
                 sh("docker rmi -f vivek796/petclinic-jenkins-jfrog:latest || :")
                 sh("docker rmi -f vivek796/petclinic-jenkins-jfrog:$SHORT_COMMIT || :")
             }
         }*/


    }

    post {
        changed {
            script {
                //currentBuild.result = "FAILURE";
                // set variables
                def subject = "${env.JOB_NAME} - Build #${env.BUILD_NUMBER} ${currentBuild.result}"
                def content = '${JELLY_SCRIPT,template="html"}'

                if (currentBuild.currentResult == 'FAILURE') { // Other values: SUCCESS, UNSTABLE
                    // Send an email only if the build status has changed from green/unstable to red
                    echo '=== Inosde the post FAILURE ==='
                    emailext subject: subject ,
                            body: content,
                            recipientProviders: [
                                    [$class: 'CulpritsRecipientProvider'],
                                    [$class: 'DevelopersRecipientProvider'],
                                    [$class: 'RequesterRecipientProvider']
                            ],
                            replyTo: '$DEFAULT_REPLYTO',
                            to: '$DEFAULT_RECIPIENTS'
                } else if (currentBuild.currentResult == 'SUCCESS') { // Other values: SUCCESS, UNSTABLE
                    // Send an email only if the build status has changed from green/unstable to red
                    echo '=== Inosde the post SUCCESS ==='
                    emailext subject: subject,
                            body: content,
                            recipientProviders: [
                                    [$class: 'CulpritsRecipientProvider'],
                                    [$class: 'DevelopersRecipientProvider'],
                                    [$class: 'RequesterRecipientProvider']
                            ],
                            replyTo: '$DEFAULT_REPLYTO',
                            to: '$DEFAULT_RECIPIENTS'
                }
            }
        }
    }


}


