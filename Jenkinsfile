// pipeline {
//         agent {
//             docker {
//                 image 'node:16-buster-slim' 
//                 args '-p 3000:3000' 
//             }
//         }
//         stages {
//             stage('Build') { 
//                 steps {
//                     sh 'npm install'
//                 }
//             }
//             stage('Test') {
//                 steps {
//                     sh './jenkins/scripts/test.sh'
//                 }
//         }
//     }
// }

properties([
    pipelineTriggers([
        pollSCM('H/2 * * * *') // Poll setiap 2 menit
    ])
])

node {
    docker.image('node:16-buster-slim').inside('-p 3000:3000') {
        stage('Checkout') {
            checkout([$class: 'GitSCM', branches: [[name: '*/react-app']], userRemoteConfigs: [[url: '/home/dicoding/devops-intermediete/a428-cicd-labs']]])
        }
        stage('Build') {
            sh 'npm install'
            echo 'Build stage completed successfully!'

        }
        stage('Test') {
            sh './jenkins/scripts/test.sh'
        }
        stage('Manual Approval') {
            input message: 'Lanjutkan ke tahap Deploy? (Klik "Proceed" untuk melanjutkan)'
        }
        stage('Deploy') {
            sh './jenkins/scripts/deliver.sh'
            sleep(time:1, unit:"MINUTES")
            sh './jenkins/scripts/kill.sh'
        }
    }
}

//test scm 2 last try