#!/usr/bin/env groovy

node {
    def branch = env.BRANCH_NAME
	def imageName = 'user-management-viennguyen'
	def imageTag = ''
    
    stage('HELLO PIPELINE') {
        println "Hello, this is my first pipeline!"
        println "Current branch: $branch"
    }

    stage('PREPARATION') {
        // Clean up docker images which were built before
        sh returnStatus: true, script: 'docker rmi $(docker images -a -q)'
        // Setup tools: Java, maven
        env.JAVA_HOME="${tool 'Java8'}"
		env.PATH="${env.JAVA_HOME}/bin:${env.PATH}"
		sh 'java -version'

        // Checkout branch
		// Need to set LocalBranch extension, since: https://stackoverflow.com/questions/44006070/jenkins-gitscm-finishes-the-clone-in-a-detached-head-state-how-can-i-make-sure
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
    }

    stage('BUILD PROJECT') {
		//execute maven build
        sh returnStatus: true, script: '''cd user-management
            ./mvnw clean install'''
	}
}

// GENERAL HELPERS

String buildImageTagFromPomFile(String branch) {
	def artifactVersion = fileExists('pom.xml') ? readMavenPom(file: 'pom.xml').version : ''
	artifactVersion = artifactVersion - '-SNAPSHOT'
	def gitRev = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()

	return "$artifactVersion-$branch-$gitRev"
}