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
            agent {
                docker {
                    image 'maven:3-alpine'
                    args '-v /root/.m2:/root/.m2'
                    label 'master'
                }
            }
            stage('Build') {
                steps {
                    sh 'mvn -B -DskipTests clean package'
                }
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
