node{
    stage('pull code'){
        notifyMe('#00EE00',":jenkins: @ryan *STARTED:* Job <${env.JOB_URL}|${env.JOB_NAME}> Build #<${env.BUILD_URL}|${env.BUILD_NUMBER}>")
        git url: 'https://github.com/ryantakeuchi/docker-hello-world.git' // Pull code from SCM
    }

    stage('build container'){
        sh 'docker build -t web .'
    }

    stage('appprove DEV deploy'){
        notifyMe('#FFFF00',":hi: Hello @ryan! *APPROVAL REQUIRED* to deploy ${env.JOB_NAME} to DEV")
        timeout(time: 1500, unit: 'SECONDS') { // automatically fail stage if no manual input
            userInput = input(
            id: 'deployDev', message: 'Deploy to Dev?', ok: 'Deploy'
            )
        }
    }

    stage('deploy to dev'){
        sh 'docker-compose up -d'
        notifyMe('#ADFF2F',"Successfully deployed ${env.JOB_NAME} to DEV! Notify @ryan")
    }

    stage('clean up DEV'){
        timeout(time: 1500, unit: 'SECONDS') { // automatically fail stage if no manual input
            userInput = input(
            id: 'cleanDev', message: 'Delete DEV environment?', ok: 'DELETE'
            )
        sh '''
        docker stop $(docker ps -a -q)
        docker rm $(docker ps -a -q)
        '''
        }
    }

    stage('approve QA deploy'){
        notifyMe('#FFFF00',":hi: Hello @ryan! *APPROVAL REQUIRED* to deploy ${env.JOB_NAME} to QA")
        timeout(time: 1500, unit: 'SECONDS') { // automatically fail stage if no manual input
            userInput = input(
            id: 'deployQA', message: 'Deploy to QA?', ok: 'Deploy'
            )
        }
    }

    stage('deploy to QA'){
        sh 'docker run -d -p 8000:80 web'
        notifyMe('#ADFF2F',"@ryan Successfully deployed ${env.JOB_NAME} to QA! Notify @ryan")
    }

    stage('clean up QA'){
        timeout(time: 1500, unit: 'SECONDS') { // automatically fail stage if no manual input
            userInput = input(
            id: 'cleanQA', message: 'Delete QA environment?', ok: 'DELETE'
            )
        sh '''
        docker stop $(docker ps -a -q)
        docker rm $(docker ps -a -q)
        '''
        }
    }
}

def notifyMe(thisColor,thisMsg) {
    slackSend (color: thisColor, message: thisMsg)
    }
