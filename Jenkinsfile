node {
        stage("Nettoyage") {
            // On nettoie le workspace pour être sûr de ne pas avoir de reliquat des précédents builds.
            cleanWs()
        }
        
        stage("Récupération des sources") {
            // Maintenant que notre workspace est propre on télécharge les sources du projet
            git "https://github.com/deather/jenkins-exercise-3.git"
        }    
        
        stage("Modification de la configuration") {
        // On récupère les credentials que l'on a créé au préalable dans Jenkins (http://localhost:8080/credentials/)
        // Ici c'est prod_user, prod_database et prod_password qui sont les valeurs contenues dans env/integration.properties
        // Pour rappel l'avantage des credentials est de sécuriser et centraliser les données sensibles qui peuvent être contenues dans une application
            withCredentials([
                string(credentialsId: params.user, variable: 'user'), 
                string(credentialsId: params.database, variable: 'database'), 
                string(credentialsId: params.password, variable: 'password')
            ]) {
                // On lit le contenu du fichier conf/bdd.conf
                // afin de pouvoir remplacer les valeurs null par celle de l'environement d'integration
                // La configuration de la bdd est stoquée au format JSON
                String config = readFile "conf/bdd.conf"
                def configJson = new groovy.json.JsonSlurperClassic().parseText(config)
                
                configJson.user = user
                configJson.database = database
                configJson.password = password
                
                // Maintenant que l'on a fait les modifications du json on les sauvegarde dans le fichier
                writeFile file: "conf/bdd.conf", text: groovy.json.JsonOutput.prettyPrint(groovy.json.JsonOutput.toJson(configJson))
            }
        }
        
        stage("Stockage de la configuration modifiée") {
            // On précise à jenkins d'archiver tous les fichiers qui sont dans conf/
            // Vous pourrez retrouver l'archive sur la page du job pour vérifier que cela à bien fonctionné
            archiveArtifacts 'conf/*'
        }
    }
