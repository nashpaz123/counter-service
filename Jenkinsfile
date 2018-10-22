pipeline {
    agent { label 'aws_ubuntu16' }
    stages {

        stage ("1. build image, set in repository"){
            steps {
                script {
                    sh """pwd
                          ls -l
                          
                          cd app                          
                          docker build -t counter_1.0 . 
                          """
                }
            }
        }

        stage ("2. stop existing containers"){
            steps {
                script {
                    try {
                    sh """pwd
                          ls -l
                          
                          docker stop `docker ps -a -q --filter ancestor=counter_1.0` 
                          docker ps -a
                          """
                    } catch (Exception e) {
                        echo 'nothing gained nothing lost'
                    }
                    }
                }
            }
        }

        stage ("3. run"){
            steps {
                script {
                    sh """pwd
                          ls -l
                          
                          docker run -d -p 80:80 counter_1.0
                          docker ps -a
                          """
                }
            }
        }
    }
}