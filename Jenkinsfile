#!/usr/bin/env groovy

node {

		stage('PREPARATION') {
      // TOTO: clean up docker images which were built before
      // IDEA: use 'Shell Script' step to remove all docker images
      sh 'docker rm $(docker images -a -q)'

      // TODO: setup tools: Java, Maven...
      // IDEA: use 'Tool' step to get path of installed Java, then set Java path into env.PATH

      // TODO: checkout project, please remember to checkout only your branch!
      // IDEA: use 'Checkout' step

      // TODO: build image tag, later we will use this tag to tag docker image in this build
      // IDEA: some of global variables that might interesting!

      // TODO: may be change display name of this build to display image tab
    }

} 