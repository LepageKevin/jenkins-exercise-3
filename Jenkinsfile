Jenkinsfile (Declarative Pipeline)
pipeline {
    agent any

    parameters {
        string(name: 'environnement', defaultValue: 'Integration', description: 'Variable permettant de savoir dans quel environnement on se trouve pour remplir le fichier')
    }

    stages {
	    stage("Cleans") {
	        // On clean le workspace
	        cleanWs()
	    }

	    stage("Git") {
	        // On git clone
	        git 'https://github.com/deather/jenkins-exercise-3'
	    }   
	    stage("Integration") {
	        //Credentials
	        withCredentials([
	            string(credentialsId: 'user' + ${params.environnement}, variable: 'user'),
	            string(credentialsId: 'database' + ${params.environnement}, variable: 'database'),
	            string(credentialsId: 'password'+ ${params.environnement}, variable: 'password')]) {
	                

	            String configFile = readFile 'conf/bdd.conf'
	            def result = new groovy.json.JsonSlurperClassic().parseText(configFile)
	            result.user = user
	            result.database = database
	            result.password = password
	            
	            def json = new groovy.json.JsonOutput().toJson(result)
	            writeFile file: "conf/bdd.conf", text: json
	        }
	    }
    }
}