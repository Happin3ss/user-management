#!/usr/bin/env groovy

def imageTag =''
node {
    stage('PREPARATION') {
    		// TOTO: clean up docker images which were built before
    		sh "docker image prune -fa"

            env.JAVA_HOME = "${tool name: 'Java8', type: 'jdk'}"
            println "$env.JAVA_HOME"
            env.PATH = "$env.JAVA_HOME/bin:$env.PATH"

    		checkout poll: false, scm: [$class: 'GitSCM', branches: [[name: '*/vvhoang']],
                     doGenerateSubmoduleConfigurations: false, extensions: [],
                     submoduleCfg: [],
                     userRemoteConfigs: [[credentialsId: 'tranductrinh', url: 'https://github.com/tranductrinh/user-management']]]

            imageTag = buildImageTagFromPomFile('vvhoang')

    		currentBuild.displayName = imageTag
    }
    stage('BUILD PROJECT') {
    		sh './mvnw clean install'
    	}
}
String buildImageTagFromPomFile(String branch) {
	def artifactVersion = fileExists('pom.xml') ? readMavenPom(file: 'pom.xml').version : ''
	artifactVersion = artifactVersion - '-SNAPSHOT'
	def gitRev = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()

	return "$artifactVersion-$branch-$gitRev"
	}