# Objectifs
Conteneuriser une application de vote avec plusieurs services, le tout orchestré par un docker-compose
#Consignes Dockerfiles
Vous devez créer 3 images.
Les spécifications de chaque image sont décrites ci-dessous.

# Poll
L'image doit être basée sur une image officielle de Python.
Les dépendances de l'application doivent être installées.

L'application doit s'exposer et s'exécuter sur le port 80, et peut être démarrée avec :
flask run --host=0.0.0.0 --port=80

# Result
L'image doit être basée sur une image officielle Node.js version 20 Alpine.
Les dépendances de l'application doivent être installées.

Le répertoire node_modules doit être exclu du contexte de construction.

L'application doit s'exposer et s'exécuter sur le port 80.

# Worker
L'image sera construite en utilisant une construction en plusieurs étapes.

Première étape - build
La première étape doit être basée sur maven:3.9.6-eclipse-temurin-21-alpine et être nommée builder.
Elle doit être utilisée pour construire (bien sûr) et empaqueter l'application Worker en utilisant les commandes suivantes
suivantes :

mvn dependency:resolve, depuis le répertoire contenant pom.xml.
puis, mvn package, à partir du répertoire contenant le répertoire src.
Cela génère un fichier dans le répertoire cible nommé worker-jar-with-dependencies.jar (relatif à votre répertoire WORKDIR).

 Deuxième étape - exécution
La deuxième étape doit être basée sur eclipse-temurin:21-jre-alpine.
C'est celle qui exécute réellement le worker en utilisant la commande :
java -jar worker-jar-with-dependencies.jar

# Consignes Docker compose
Vous avez maintenant 3 Dockerfiles qui créent 3 images isolées.
Il est maintenant temps de les faire fonctionner ensemble en utilisant Docker Compose !
Créez un fichier docker-compose.yml qui sera responsable de l'exécution et de la liaison de vos conteneurs.
Votre fichier Docker Compose doit contenir :

# 5 services
# poll
Construit votre image de poll.
Redirige le port 5000 de l'hôte vers le port 80 du conteneur.
Définit correctement la variable d'environnement nécessaire.

# redis
Utilise une image officielle existante de Redis 7.
Ouvre le port 6379.

# worker
Construit votre image de worker
Définit correctement les mêmes variables d'environnement que les services poll et result.

# db
Représente la base de données qui sera utilisée par les applications.
Utilise une image officielle existante de PostgreSQL 16.
A son schéma de base de données créé lors du premier démarrage du conteneur.
Définit correctement les variables d'environnement appropriées.

# result
Construit votre image de result.
Redirige le port 5001 de l'hôte vers le port 80 du conteneur.
Définit correctement les variables d'environnement nécessaires.

# 3 réseaux
poll-tier pour permettre le service poll de communiquer avec redis.
result-tier pour permettre le service result de communiquer avec db.
back-tier pour permettre le service worker de communiquer avec redis et db.

# 1 volume
db-data pour permettre aux données de la base de données d'être persistantes, si le conteneur s'éteint.

# Attention
Il est interdit d'utiliser l'instruction ENTRYPOINT dans les Dockerfile.
Il est également interdit d'utiliser les links dans le docker-compose

# Architecture du projet
Vous pouvez push les fichiers donnés en ressources, ce n'est pas obligatoire, mais dans un portfolio c'est plus fonctionnel avec toute l'application :)

.
├── docker-compose.yml
├── schema.sql
├── poll/
|    |   ...
|    └── Dockerfile
├── result/
|    |   ...
|    └── Dockerfile
└── worker/
     |   ...
     └── Dockerfile
