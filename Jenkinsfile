pipeline {
    agent {label "private_slave"}
    
    environment{
        rds_host = "mydb.cgcnycvwq5xu.eu-west-1.rds.amazonaws.com"
        rds_port = 3306
        redis_host = "cluster-example.egwlof.0001.euw1.cache.amazonaws.com"
        redis_port = 6379
    }

    stages {

    stage('Git Code') {
        steps {
            git "https://github.com/mostafahassan097/jenkins_nodejs_example-rds_redis"
        }
    }



    stage('Build Image') {
        steps {
            sh "docker build -f dokcerfile -t mostafahassan/node-js:v5.0"
        }
    }

    stage('Push Image') {
        steps {
            withCredentials([usernamePassword(credentialsId: 'docker-id', usernameVariable: "user", passwordVariable: "pass")]){
                sh """
                docker login -u ${user} -p ${pass}
                docker push mostafahassan/node-js:v5.0
                """
            }
        }
    }


    stage('Deploy Application ') {
        steps {

              withCredentials([usernamePassword(credentialsId: 'rds-creds', usernameVariable: "user", passwordVariable: "pass")]){
                    sh "docker run -p 80:3000 -d  --env REDIS_PORT=${redis_port} --env REDIS_HOSTNAME=${redis_host}  --env RDS_PORT=${rds_port} --env RDS_HOSTNAME=${rds_host} --env RDS_USERNAME=${user} --env RDS_PASSWORD=${pass}  mostafahassan/node-js:v5.0"

            }

            }
        }
    }
    }
