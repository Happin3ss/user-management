#!/usr/bin/env groovy
node {
    stage('PREPARATION') {
    		// TOTO: clean up docker images which were built before
    		sh "docker image prune -fa"
    		// IDEA: use 'Shell Script' step to remove all docker images

            env.JAVA_HOME = "${tool name: 'Java8', type: 'jdk'}"
            println "$env.JAVA_HOME"
            env.PATH = "$env.JAVA_HOME:$env.PATH"
    		// TODO: setup tools: Java, Maven...
    		// IDEA: use 'Tool' step to get path of installed Java, then set Java path into env.PATH

    		checkout poll: false, scm: [$class: 'GitSCM', branches: [[name: '*/vvhoang']],
                     doGenerateSubmoduleConfigurations: false, extensions: [],
                     submoduleCfg: [],
                     userRemoteConfigs: [[credentialsId: 'tranductrinh', url: 'https://github.com/tranductrinh/user-management']]]

    		// TODO: build image tag, later we will use this tag to tag docker image in this build
    		// IDEA: some of global variables that might interesting!

    		// TODO: may be change display name of this build to display image tab
    }
}