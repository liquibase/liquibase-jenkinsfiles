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
    BASEDIR="{params.BASEDIR}"
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
        sh '''
          { set +x; } 2>/dev/null
          git clone ${GITURL}
          cd ${PROJECT}
          git checkout $BRANCH
          git status
          '''
      } // steps for checkout stages
    } // stage 'checkout'

   stage ('liquibase commands'){
      steps {
        sh '''
          { set +x; } 2>/dev/null
          export LIQUIBASE_ARGS="--defaultsFile={ENVIRONMENT}/liquibase.properties --changelogFile=${CHANGLOGFILE} --classpath=${CLASSPATH}"
          cd ${PROJECT}/${BASEDIR}
          liquibase --version
          liquibase $LIQUIBASE_ARGS status --verbose
          liquibase --url=${URL} --password=${PASSWORD} --contexts=$ENVIRONMENT_STEP rollbackCount 2
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