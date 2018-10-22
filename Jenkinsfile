pipeline {
    agent { label 'aws_ubuntu16' }

    parameters {choice(name: 'Build_Image', choices: "Dont_build\nYey", description: 'Do you wannna build a snow ma.. I mean, run the build tag push stage')}

    stages {
        stage ("1. Build image, tag and set in repo"){
            when { // Only builds if Build_Image is required
                expression { params.Build_Image == 'Yey' }
            }
                steps {
                    script {
                        sh """pwd
                              ls -l
                              
                              cd app                          
                              docker build -t counter_1.0.1 . 
                              docker tag counter_1.0.1 nash/repo1:tag_1.0.1
                              #The next line (push to docker.io) won't run in dev. Set a when { branch 'master' } for it  
                              #docker push nash/repo1:tag_1.0.1
                              """
                    }
                }
            }

        stage ("2. Stop existing, prep redis"){
            steps {
                script {
                    try {
                    sh """pwd
                          ls -l
                          
                          docker rm `docker ps -a |grep redis | awk '{print \$1}'`
                          docker stop `docker ps -a |grep nash | awk '{print \$1}'`
                          docker run --name redis -d redis
                          docker ps -a
                          """
                    } catch (Exception e) {
                        echo 'nothing to stop, nothing gained nothing lost'
                    }

                }
            }
        }

        stage ("3. Deploy"){
            steps {
                docker run --name redis -d redis
                script {
                    sh """pwd
                          ls -l

                          docker run --name web --link redis:redis -p 5000:80 -d nash/repo1:tag_1.0.1 python app.py
                          docker ps -a
                          """
                }
            }
        }

        stage ("3. Test"){
            steps {
                script {
                    sh """                    
                          curl http://localhost:80
                          """
                }
            }
        }
    }
}