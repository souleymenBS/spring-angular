pipeline {
    agent any
    tools {
        maven 'maven'
        nodejs 'node_14'
    }
    stages {
        stage ("clean up") {
            steps {
                deleteDir()
            }
        }
        stage ("clone backend repo") {
            steps {
                sh "git clone https://github.com/souleymenBS/back-spring.git springboot/app"
            }
        }
        stage ("clone frontend repo") {
            steps {
                sh "git clone https://github.com/souleymenBS/back-spring.git angular-app"
            }
        }
        stage ("install Angular dependencies") {
            steps {
                dir("angular-frontend") {
                    sh "npm install"
                }
            }
        }
        stage ("build Angular project") {
            steps {
                dir("angular-frontend") {
                    sh "ng build --prod"
                }
            }
        }
        stage ("generate backend image") {
            steps {
                dir("springboot/app") {
                    sh "mvn clean install"
                    sh "docker build -t spring-angular ."
                }
            }
        }
        stage ("generate frontend image") {
            steps {
                dir("angular-app") {
                    sh '''docker build -t angular-frontend -f Dockerfile .'''
                }
            }
        }
        stage ("run docker compose") { 
            steps {
                dir("spring-angular") {
                    sh "docker compose up -d"
                }
            }
        }
    }
}
