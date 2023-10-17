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
                sh "git clone https://github.com/souleymenBS/spring-angular.git"
            }
        }
       
        stage ("install Angular dependencies") {
            steps {
                dir("spring-angular/angular-app") {
                    sh "npm install"
                    sh "npm run build"
                    sh "docker build -t frontapp ."
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
                dir("spring-angular") {
                    sh "docker compose up -d"
                }
            }
        }
    }
}
