pipeline {
    agent any
    tools{
        nodejs 'mynode'
    }
    stages {
        stage('git clone') {
            steps {
                echo 'cloning repo from github'
            checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Prasad-bhoite19/Jenkins-Node-App.git']])
        echo "cloned successfully...."
            }
        }
       stage('Build') {
            steps {
                echo 'Building myapp'
                sh 'npm install'
                echo 'Build successfully....'
            }
        }
       stage('Test') {
            steps {
                echo 'Testing myapp'
                sh './node_modules/mocha/bin/_mocha --exit ./test/test.js'
            }
        }
        stage('Deploy') {
    steps {
        script {
            echo 'Deploying myapp on live server'

            sshagent(['My-Ubuntu']) {
                sh '''
                ssh -o StrictHostKeyChecking=no ubuntu@51.21.180.69 <<EOF
            

                  mkdir -p nodeapp
                  cd nodeapp

                  if [ ! -d .git ]; then
                    git init
                    git remote add origin https://github.com/Prasad-bhoite19/Jenkins-Node-App.git
                  fi

                  git pull origin master

                  sudo apt update
                  sudo apt install -y nodejs npm

                  npm install
                  sudo npm install -g pm2

                  pm2 restart nodeapp || pm2 start app.js --name nodeapp
                  pm2 save
                  exit
                EOF
                '''
            }
        }
    }
}
}
}
