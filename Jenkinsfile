#!/use/bin/env groovy

node {
    
    stage('HELLO') {
        println "HELLO WORLD"
    }

    stage('Preparation') {
        // TOTO: clean up docker images which were built before
		// IDEA: use 'Shell Script' step to remove all docker images

 		// TODO: setup tools: Java, Maven...
		// IDEA: use 'Tool' step to get path of installed Java, then set Java path into env.PATH
        env.JAVA_HOME = "${tool 'Java8'}"

        println "${env.JAVA_HOME}"
        println "${env.PATH}"
        env.PATH += ":${env.JAVA_HOME}"
        println "${env.PATH}"

 		// TODO: checkout project, please remember to checkout only your branch!
		// IDEA: use 'Checkout' step

 		// TODO: build image tag, later we will use this tag to tag docker image in this build
		// IDEA: some of global variables that might interesting!

 		// TODO: may be change display name of this build to display image tab
    }
}