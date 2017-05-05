node {
    // On nettoie le workspace pour être sûr de ne pas avoir de reliquat des précédents builds.
    cleanWs()

    // Maintenant que notre workspace est propre on télécharge les sources du projet
    git "https://github.com/deather/jenkins-exercise-3.git"

    echo params.user
    echo params.database

    // On récupère les credentials que l'on a créé au préalable dans Jenkins (http://localhost:8080/credentials/)
    // Pour rappel l'avantage des credentials est de sécuriser et centraliser les données sensibles qui peuvent être contenues dans une application
    //
    // Vous remarquerez que la diffèrence avec l'exercice précédent est que l'on a variabilisé les credentialsId
    // ce qui a pour effet que l'on a le même script pour le job de production et d'intégration
    withCredentials([string(credentialsId: params.user, variable: 'user'), string(credentialsId: params.database, variable: 'database'), string(credentialsId: params.password, variable: 'password')]) {
        // On lit le contenu du fichier conf/bdd.conf
        // afin de pouvoir remplacer les valeurs null par celle de l'environement de production
        // La configuration de la bdd est stoquée au format JSON
        String config = readFile "conf/bdd.conf"

        echo user
        echo database
        echo password

        // La méthode replace renvoie une copie de la chaîne remplacée
        // d'où le config = config.replace()
        // https://docs.oracle.com/javase/8/docs/api/java/lang/String.html#replace-java.lang.CharSequence-java.lang.CharSequence-
        config = config.replace("[user]", user)
        config = config.replace("[database]", database)
        config = config.replace("[password]", password)

        // Maintenant que l'on a fait les modifications du json on les sauvegarde dans le fichier
        writeFile file: "conf/bdd.conf", text: config
    }

    // On précise à jenkins d'archiver tous les fichiers qui sont dans conf/
    // Vous pourrez retrouver l'archive sur la page du job pour vérifier que cela à bien fonctionné
    archiveArtifacts 'conf/*'
}