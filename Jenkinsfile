pipeline {
    agent any
    parameters {
        string(name: 'uname', defaultValue: 'chetan')
        password(name: 'token', defaultValue: '')
        text(name: 'functionList', defaultValue: '')
    }
    stages {
        stage('Checkout') {
            steps {
                git branch:'main', credentialsId:'chetanGithub', url:'https://github.com/chetangaikar/lambdascripts.git'
            }
        }
        stage('Call Other Pipelines') {
            steps {
                sh '''
                  hash=$(git rev-parse refs/remotes/origin/main^{commit})
                  files=$(git show --pretty="" --name-only $hash)
                  for fileName in $files
                  do
                    if [ $fileName = "Jenkinsfile" ]
                    then continue
                    fi
                    for functionName in $functionList
                      do
                        if [ $fileName = "$functionName/lambda_function.py" ]
                        then
                          echo "\n\nDeploying $functionName\n\n"
                          curl -X POST -u $uname:$token http://localhost:8080/job/$functionName/build
                        fi
                      done
                  done
                '''
            }
        }
    }
}
