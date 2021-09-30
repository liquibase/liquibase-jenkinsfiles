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
    PATH="/root/liquibase:$PATH"
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
          echo "PATH="$PATH
          export LIQUIBASE_DEFAULTS_FILE="${ENVIRONMENT}/liquibase.properties"
          export LIQUIBASE_CHANGELOG_FILE="${CHANGLOGFILE}"
          export LIQUIBASE_CLASSPATH="${CLASSPATH}"
          #export LIQUIBASE_ARGS="--defaultsFile=${ENVIRONMENT}/liquibase.properties --changelogFile=${CHANGLOGFILE} --classpath=${CLASSPATH}"
          export LIQUIBASE_ARGS=""
          #export "LIQUIBASE_ARGS="$LIQUIBASE_ARGS
          cd ${PROJECT}/${BASEDIR}
          ls -alh
          liquibase --version
          liquibase $LIQUIBASE_ARGS status --verbose
          liquibase $LIQUIBASE_ARGS updateSQL
          liquibase $LIQUIBASE_ARGS update
          liquibase $LIQUIBASE_ARGS history
        '''
      } // steps
    }   // Environment stage

        stage ('Deleting project workspace'){
           steps {
             sh '''
               { set +x; } 2>/dev/null
               echo "Deleting project workspace..."
               cd /localPath/to/workspaceFolder && rm -r ${PROJ}
             '''
           } // steps
         }   // Deleting project workspace
  } // stages
}  // pipeline