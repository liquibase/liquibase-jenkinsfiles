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
          export LIQUIBASE_PRO_LICENSE_KEY=ABwwGgQUWhUekpYqsouociHYaajSx3x+R4cCAgQAlod+R+bA6fJ9PGJTTDO7uSbKrCwHO+y8+SILUI8m8p9O/BHtoYlT6+dh6PlA7DQAsZirRUBrbDcc4YC4ph5/rmdjgcQLNQnYAUGc0JPFD1RgpdM2OS8dFOfjO7IinK8xlHih2aumzqBl4nh95vkEfaV7pEHmA2yb1EHOOC42XB//o8vvQ+Ji/hAp0SYKoIGjeb5XEuUSD7G8LSRDx4/AuhKdXECeLwxxwe3hBIzZBAXpto68cS+EOexSsVTrov2Z/hD2fasudc9xbmlfHv6heDFHXx8hSK2R+2fxvw9IddZLQ1gZbExFl+tW+0XgeuiJaZKi3bfUZ0+qIeXNogVW0GBO0tWX5/0RIT3yGx405157Dw1cixNiePJZUcfVxtJZ6FNbYH2C7SHsJo5rli1Fp+0CgTIviX+x6O2v6P6JDvxnEz3qkIflOafXsJGsD6e1xW+ohrvD5P+4dQHTQkeqXbpX6C2my4mnW5wZE2E5arEVdT4ftHjRHQKLzKJDyDUjW1yz9mC8YlHSXCLnMYWlrnsDd8sjruL2ExUEdGjvmAoKOLUr9DUu8frXt53rbx8GHVSRxY1JxlfZjYbjEMVY3C2XuZSyO+CRUwkI0+xwsKvf6kKUKnOQyMV4MONhCGSM+7SaJ0TKxJu6L0Gb2MDo+Mh/lWO4H042
          export LIQUIBASE_DEFAULTS_FILE=${ENVIRONMENT}/liquibase.properties
          export LIQUIBASE_COMMAND_CHANGELOG_FILE=${CHANGELOGFILE}
          export LIQUIBASE_CLASSPATH=${CLASSPATH}
          #export LIQUIBASE_ARGS="--defaultsFile=${ENVIRONMENT}/liquibase.properties --changelogFile=${CHANGLOGFILE} --classpath=${CLASSPATH}"
          export LIQUIBASE_ARGS=""
          #export "LIQUIBASE_ARGS="$LIQUIBASE_ARGS


          cd ${PROJECT}/${BASEDIR}
          ls -alh
          echo "LIQUIBASE_COMMAND_CHANGELOG_FILE="${LIQUIBASE_COMMAND_CHANGELOG_FILE}
          echo "LIQUIBASE_COMMAND_DEFAULTS_FILE="${LIQUIBASE_COMMAND_DEFAULTS_FILE}

          liquibase --version
          liquibase $LIQUIBASE_ARGS status --verbose
          liquibase $LIQUIBASE_ARGS updateSQL
          #liquibase $LIQUIBASE_ARGS update
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