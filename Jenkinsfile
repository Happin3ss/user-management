#!/usr/bin/env groovy

node {
    stage('Preparation') {
        // TOTO: clean up docker images which were built before
        // IDEA: use 'Shell Script' step to remove all docker images
        sh returnStatus: true, script: 'docker rmi -f $(docker images -q)'

        // TODO: setup tools: Java, Maven...
        // IDEA: use 'Tool' step to get path of installed Java, then set Java path into env.PATH
        env.JAVA_HOME="${tool 'Java8'}"
        env.PATH="${env.JAVA_HOME}/bin:${env.PATH}"
        sh 'java -version'

        // TODO: checkout project, please remember to checkout only your branch!
        // IDEA: use 'Checkout' step
        checkout([$class: 'GitSCM',
                  branches: [
                          [
                                  name: "*/$branch"
                          ]
                  ],
                  doGenerateSubmoduleConfigurations: false,
                  extensions: [],
                  extensions: [
                          [$class: 'CleanBeforeCheckout'],
                          [$class: 'LocalBranch', localBranch: "**"]
                  ],
                  submoduleCfg: [],
                  userRemoteConfigs: [
                          [
                                  credentialsId: 'ci-user-ssh',
                                  url: 'git@github.com:tranductrinh/user-management.git'
                          ]
                  ]
        ])

        imageTag = buildImageTagFromPomFile(branch)

        // Change build name
        currentBuild.displayName = imageTag

        // TODO: build image tag, later we will use this tag to tag docker image in this build
        // IDEA: some of global variables that might interesting!
        stage('BUILD PROJECT') {
            // TODO: execute maven build
            // IDEA: use 'Shell Script' step, and also see README.md - how to build project
        }

        // TODO: may be change display name of this build to display image tab
    }
}
// GENERAL HELPERS

String buildImageTagFromPomFile(String branch) {
    def artifactVersion = fileExists('pom.xml') ? readMavenPom(file: 'pom.xml').version : ''
    artifactVersion = artifactVersion - '-SNAPSHOT'
    def gitRev = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()

    return "$artifactVersion-$branch-$gitRev"
}