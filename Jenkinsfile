pipeline {
    agent any 
    environment {
       DOCKER = tool 'docker' 
       DOCKER_EXEC = '$DOCKER/docker'
       SCA = '/Users/clagosu/Documents/dependency-check/bin/dependency-check.sh'
    }
    stages {
      
      stage('SCM') {
         steps {
            figlet 'SCM'
            checkout scm // clonacion de codigo en nodo
         }
      }
        
      stage('BUILD') {
         steps {
            figlet 'BUILD'
            sh 'set +x; chmod 777 gradlew'
            sh './gradlew clean build'
          //archiveArtifacts artifacts: "build/libs/testing-web-*.jar"
         }
      }
        
      stage('SAST') {
         steps {
            withCredentials([string(credentialsId: 'sonarcloud', variable: 'SONARPAT')]) {
                 figlet 'SAST'
                 sh('set +x; ./gradlew sonarqube -Dsonar.login=$SONARPAT -Dsonar.branch.name=feature-jenkins')
            }
         }
      }
        
      stage('SCA') {
        steps {
            figlet 'SCA'
            sh "$SCA --project 'spring-clinic' --scan '${WORKSPACE}/build/libs/pet-clinic-2.6.0.jar'"
            echo '************** SBOM CYCLONEDX **************'
            sh "./gradlew cyclonedxBom -info"
        }
     }
        
      stage('Build Image') {
         steps {
            figlet 'IMAGE'
          //sh '${DOCKER EXEC} build .'
            //sh "$DOCKER_EXEC images"
          //sh "$DOCKER_EXEC push clagosu/spring-clinic:$currentBuild.number"
            }
        }
        
        stage('DAST') {
        steps {
            figlet 'DAST'
            //sh "${DOCKER_EXEC} run --rm -v ${WORKSPACE}:/zap/wrk/:rw -t owasp/zap2docker-stable zap-baseline.py -t https://www.hackthissite.org/ -r DAST_Report.html"
        }
     }
        
   }
}

