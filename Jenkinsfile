pipeline {
    agent any
    
    tools {
        ant     "${env.ANT_TOOL}"
        gradle  "${env.GRADLE_TOOL}"
        nodejs  "${env.NODE_TOOL}"
    }

    stages {
        stage('Prepare runtime parameters') {
            steps {
                script {
                    echo "Preparing runtime parameters..."
                    
                    publish_path_prefix = "${env.PUBLISH_ROOT_PATH}/" + env.JOB_BASE_NAME.substring(env.JOB_BASE_NAME.lastIndexOf('-') + 1, env.JOB_BASE_NAME.length())
                    echo "publish_path_prefix: [${publish_path_prefix}]"
                }
            }
        }
        
        stage('Build') {
            steps {
                echo "Building ${env.JOB_NAME}..."
                sh 'gradle clean build'
            }
        }

        stage('Publish') {
            steps {
                echo "rm -rf ${publish_path_prefix}/apps/NorthAlarmService/*"
                sh "rm -rf ${publish_path_prefix}/apps/NorthAlarmService/*"
                
                echo "mkdir -p ${publish_path_prefix}/apps/NorthAlarmService/"
                sh "mkdir -p ${publish_path_prefix}/apps/NorthAlarmService/"
                
                echo "cp build/libs/NorthAlarmService.jar ${publish_path_prefix}/apps/NorthAlarmService/"
                sh "cp build/libs/NorthAlarmService.jar ${publish_path_prefix}/apps/NorthAlarmService/"
                
                echo "rsync -r --exclude=.svn scripts/* ${publish_path_prefix}/apps/NorthAlarmService"
                sh "rsync -r --exclude=.svn scripts/* ${publish_path_prefix}/apps/NorthAlarmService"
            }
        }
    }
}