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
                dir("spring-angular/angular-app") {
                    sh 'npm install'
                }
            }
        }
        stage ("build Angular project") {
            steps {
                dir("spring-angular/angular-app") {
                    sh 'npx ng build --prod'
                }
            }
        }
        stage ("generate backend image") {
            steps {
                dir("spring-angular/springboot/app") {
                    sh "mvn clean install"
                    sh "docker build -t spring-angular ."
                }
            }
        }
        stage ("generate frontend image") {
            steps {
                dir("spring-angular/angular-app") {
                    sh '''docker build -t angular-frontend -f Dockerfile .'''
                }
            }
        }
        stage ("run docker compose") { 
            steps {
                dir("spring-angular/spring-angular") {
                    sh "docker compose up -d"
                }
            }
        }
    }
}
