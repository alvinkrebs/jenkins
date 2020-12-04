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
                    showHackFiles = {
                        it.eachDir(showHackFiles)
                        it.eachFileMatch(~/.*.hack/) {
                            f -> result += "${file.absolutePath}\n"
                        }
                    }
                    showHackFiles(new File("."))
                    println result
                }
            }
        }
        stage('Stage 4') {
            steps {
                sh """
                    mkdir hack_src
                    for i in `find . -name \\*.hack` ; do
                        echo found \$i
                        cp \$i hack_src
                    done
                    tar zcf hack_src.tgz hack_src
                    ls -lah
                """
            }
        }
    }
}
