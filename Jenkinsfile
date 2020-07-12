pipeline {

    agent { label "master" }


    stages {
        stage("test") {
            node('master') {
                checkout scm
        }


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
                    sh 'mvn -DskipTests clean package'
                }
        }
        }

        stage("build img") {
            steps {
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
                    sh 'docker stop $IMAGE || echo 'no container to stop''
                    sh 'docker rm $IMAGE || echo 'no container to remove''
                    sh 'docker run -p 8080:8080 $IMAGE'
                }
            }
        }

        stage("test-deployment") {
            steps {
                script {
                    sleep 30
                    sh "curl http://ec2-3-126-92-181.eu-central-1.compute.amazonaws.com:8080"
                }
            }
        }


    }

    }
