pipeline {
    agent any 
    stages {
        stage('Stage 1') {
            steps {
                echo 'Hello world!' 
                sh "printenv"
            }
        }
        stage('Stage 2') {
            steps {
                    checkout(
                        [
                            $class: 'GitSCM',
                            branches: [[name: '*/master']],
                            doGenerateSubmoduleConfigurations: false,
                            extensions: [],
                            submoduleCfg: [],
                            userRemoteConfigs: [[credentialsId: 'JenkinsTest', url: 'https://github.com/alvinkrebs/hhvm-fun.git']]
                        ]
                    )
            }
        }
        stage('Stage 3') {
            steps {
                script {
                    @NonCPS
                    def result
                    def here = "/var/jenkins_home/workspace/MyFirstPipeline"
                    showHackFiles = {
                        it.eachDir(showHackFiles)
                        it.eachFileMatch(~/.*.hack/) {
                            f -> result += "${file.absolutePath}\n"
                        }
                    }
                    showHackFiles(new File(here))
                    println result
                }
            }
        }
        stage('Stage 4') {
            steps {
                sh """
                    for i in `find . -name \\*.hack` ; do
                        echo found \$i
                    done
                    ls -lah
                """
            }
        }
    }
}
