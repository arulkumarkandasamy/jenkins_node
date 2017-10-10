node('master') {
          withCredentials([[$class: 'FileBinding', credentialsId: 'npmrc', variable: 'SECRET_FILE']]) {
                     echo $SECRET_FILE
                     dir('/tmp') {
                     stash name: 'npmrc_file', includes: $SECRET_FILE
                   }
              }
        }

node('windows_slave') {
/*environment {
    NODE_VERSION = '6'
    NPM_VERSION = '3'
    }
      stage('Initialize') {
        echo 'Initializing...'
        def node = tool name: 'Node-7.4.0', type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation'
        env.PATH = "${node}/bin:${env.PATH}"
        env:Path += ";C:\\Program Files\\nodejs"
    }*/

    stage('Checkout') {
        deleteDir()
        echo 'Getting source code...'
        checkout scm
    }

    stage('Install') {
          dir('C:\\jenkins') {
            unstash: 'npmrc_file'
          }

    bat """
          nvm install 6
          nvm use 6
          npm install -g npm@3
          npm install
          npm run bootstrap
          tar -czf install.tar.gz node_modules packages/*/node_modules packages/*/dist
        """
    }


    stage('Build') {
        echo 'Building dependencies...'
        bat 'npm i'
    }

    stage('Test') {
        echo 'Testing...'
        bat 'npm test'
    }

    stage('Publish') {
        echo 'Publishing Test Coverage...'
		publishHTML (target: [
			allowMissing: false,
			alwaysLinkToLastBuild: false,
			keepAll: true,
			reportDir: 'coverage/lcov-report',
			reportFiles: 'index.html',
			reportName: "Application Test Coverage"
		])
    }
}

node('staging') {
    stage('Initialize'){
        echo 'Initializing...'
        def node = tool name: 'Node-7.4.0', type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation'
        env.PATH = "${node}/bin:${env.PATH}"

        bat "node -v"

        // set environment variables
        env.VARIABLE_1="10"
        env.VARIABLE_2="7"
    }

    stage('Checkout') {
        echo 'Getting source code...'
        checkout scm
    }

    stage('PM2 Install') {
        echo 'Installing PM2 to run application as daemon...'
        bat "npm install pm2 -g"
    }

    stage('Build') {
        echo 'Building dependencies...'
        bat 'npm i'
    }

    stage('Test') {
        echo 'Testing...'
        bat 'npm test'
    }

    stage('Run Application') {
        echo 'Stopping old process to run new process...'
        bat '''
        npm run pm2-stop
        npm run pm2-start
        '''
    }
}
