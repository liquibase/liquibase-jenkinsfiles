#!/usr/bin/env groovy
// Liquibase declarative pipeline
//
//
pipeline {
agent any
  environment {
    GITURL="${params.REPO_URL}"
    PROJECT="${params.PROJECT}"
    BRANCH="${params.BRANCH}"
    ENVIRONMENT_STEP="${params.ENVIRONMENT}"
    CHANGELOGFILE="${params.CHANGELOGFILE}"
    CLASSPATH="${params.CLASSPATH}"
    BASEDIR="${params.BASEDIR}"
    PATH="/opt/liquibase-4.5.0:$PATH"
  }
  stages {

    stage ('Precheck') {
		steps {
			sh '''
        { set +x; } 2>/dev/null
        echo "Git repository: "${GITURL}
        echo "Current project: "${PROJECT}
        echo "Current branch: "${BRANCH}
        echo "Current environment: "$ENVIRONMENT_STEP
        echo "Current changelog file: "${CHANGELOGFILE}
        echo "Current classpath: "${CLASSPATH}
        echo "Current base directory: "${BASEDIR}
        echo "Current path: "${PATH}
			'''
		} // steps
	} // stage 'precheck'

    stage ('Checkout') {
      steps {
        // checkout Liquibase project from repo
        checkout([
            $class: 'GitSCM', 
            branches: [[name: "*/${BRANCH}"]], 
            extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: "${PROJECT}"]],
            userRemoteConfigs: [[credentialsId: '16dc7cf1-1e5d-494b-89a0-141830d1ee70', url: "${GITURL}"]]
            ])
        sh '''
          { set +x; } 2>/dev/null
          cd ${PROJECT}
          git status
          '''
      } // steps for checkout stages
    } // stage 'checkout'

   stage ('liquibase commands'){
      steps {
	sh '''

		  { set +x; } 2>/dev/null
		  export LIQUIBASE_PRO_LICENSE_KEY=<paste_LIQUIBASE_Pro_License_Key_here>
		  export LIQUIBASE_DEFAULTS_FILE=${ENVIRONMENT_STEP}/liquibase.properties
		  export LIQUIBASE_COMMAND_CHANGELOG_FILE=${CHANGELOGFILE}
		  export LIQUIBASE_CLASSPATH=${CLASSPATH}

		  cd ${PROJECT}/${BASEDIR}
		  ls -alh
		  liquibase --version
		  liquibase status --verbose
		  liquibase updateSQL
		  #liquibase update
		  liquibase history

	'''

		      
      } // steps
    }   // Environment stage

   stage ('Package'){
      steps {
        sh '''	  
	  echo
          echo "==== Creating artifact ${BUILD_NUMBER}.zip ===="
          zip -q -r ${BUILD_NUMBER}.zip *
          mv *.zip ..
	  ls ../*.zip
 
          echo
          echo "=====FINISHED===="
        '''
      } // steps
    }   // Package stage
	  
        // stage ('Deleting project workspace'){
        //    steps {
        //      sh '''
        //        { set +x; } 2>/dev/null
        //        echo "Deleting project workspace..."
        //        cd /localPath/to/workspaceFolder && rm -r ${PROJ}
        //      '''
        //    } // steps
        //  }   // Deleting project workspace
  } // stages
}  // pipeline
