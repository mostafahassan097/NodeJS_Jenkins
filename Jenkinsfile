pipeline {
    agent {label "private_slave"}
    
    stage('Build Image') {
        steps {
            sh "docker build -f dockerfile -t mostafahassan/node-js:v3.0 ."
        }
    }

    stage('Push Image') {
        steps {
            withCredentials([usernamePassword(credentialsId: 'docker-id', usernameVariable: "user", passwordVariable: "pass")]){
                sh """
                docker login -u ${user} -p ${pass}
                docker push mostafahassan/node-js:v3.0
                """
            }
        }
    }


    stage('Deploy Application ') {
        steps {

              withCredentials([usernamePassword(credentialsId: 'RDS-ID', usernameVariable: "user", passwordVariable: "pass")]){
                    sh "docker run -p 80:3000 -d --env-file /home/ubuntu/.env   --env RDS_USERNAME=${user} --env RDS_PASSWORD=${pass}  mostafahassan/node-js:v3.0"

            }

            }
        }
    }
    }
