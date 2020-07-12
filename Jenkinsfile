pipeline {

    agent { label "master" }

    // options {
    //     ansiColor('xterm')
    // }

    stages {
        // stage("test") {
        //     node('master') {
        //         checkout scm
        // }


        stage("build") {
                docker {
                    image "maven:3-jdk-8-slim"
                    args "-v \$HOME/.m2:/root/.m2"
                    label 'master'
                }
            
            steps {
                sh 'mvn --version'
                sh 'mvn clean install -ntp -B -e'
                stash includes: 'target/demo.jar', name: 'demo-jar'
            }
        }

        stage("build img") {
            steps {
                unstash 'demo-jar'
                script {
                    env["IMAGE"] = "${env.PROJ}:${env.BRANCH_NAME}.${env.BUILD_ID}"
                    def customImage = docker.build(env["IMAGE"])
                    sh "docker save -o deployment/${env["IMAGE"]}.tar ${env["IMAGE"]}"
                }
            }
        }

        stage("deploy") {
            steps {
                script {
                    sh 'docker run '
                }
            }
        }


    }

}
