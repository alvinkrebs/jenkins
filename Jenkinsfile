pipeline {
    agent any 
    stages {
        stage('Stage 1') {
            steps {
                echo 'Hello world!' 
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
                    def result = ""
                    showHackFiles = {
                        i.eachDir(showHackFiles)
                        i.eachFileMatch(~/.*.hack/) {
                            f -> result += "${file.absolutePath}\n"
                        }
                    }
                    showHackFiles(new File("."))
                    println result
                }
            }
        }
    }
}
