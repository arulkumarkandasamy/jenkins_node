node('master') {
          withCredentials([[$class: 'FileBinding', credentialsId: 'npmrc', variable: 'SECRET_FILE']]) {
                     echo '$SECRET_FILE'
		     sh 'env'
		     sh """
 		     if [ -e .npmrc ]; then
 		       sh rm -f .npmrc
 		     fi
+		     """
                     sh 'cp $SECRET_FILE .npmrc'
                     sh 'cat .npmrc'
                     stash name: 'npmrc_file', includes: '.npmrc'
              }
        }
