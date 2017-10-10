node('master') {
          withCredentials([[$class: 'FileBinding', credentialsId: 'npmrc', variable: 'SECRET_FILE']]) {
                     echo '$SECRET_FILE'
		     sh 'env'
                     sh 'cp $SECRET_FILE .npmrc'
                     sh 'cat .npmrc'
                     //dir('/tmp') {
                      stash name: 'npmrc_file', includes: '.npmrc'
                   //}
              }
        }
