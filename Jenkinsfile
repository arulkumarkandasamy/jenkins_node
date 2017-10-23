node('master') {
          withCredentials([[$class: 'FileBinding', credentialsId: 'npmrc', variable: 'SECRET_FILE']]) {
		     sh """
                         if [ -e '.npmrc' ]; then
                            rm -f .npmrc
                         fi
                     """
		     sh 'touch .npmrc'
		     sh 'cat .npmrc'
                     sh 'cp $SECRET_FILE .npmrc'
                     sh 'cat .npmrc'
                     stash name: 'npmrc_file', includes: '.npmrc'
              }
        }
