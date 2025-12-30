🚀 Mise en place de Kafka avec Docker
📦 Lancement de Kafka et Zookeeper

Pour déployer rapidement un environnement Kafka local, ce projet s’appuie sur Docker et Docker Compose.
L’utilisation des images officielles Confluent Kafka permet de lancer Kafka et Zookeeper de manière fiable et reproductible.

<img src="images/kafka-docker.png" alt="docker-kafka" width="600"/>


🔄 Utilisation de Kafka
🧪 Commandes Kafka (CLI)

Une fois Kafka démarré, il est possible de manipuler les topics et les messages directement via les outils en ligne de commande Kafka.

▶️ Consommer des messages depuis un topic
docker exec --interactive --tty broker \
kafka-console-consumer --bootstrap-server broker:9092 --topic R2

▶️ Produire des messages vers un topic
docker exec --interactive --tty broker \
kafka-console-producer --broker-list broker:9092 --topic R2

<img src="images/consumer-producer.png" alt="kafka-console" width="600"/>
📜 Affichage de l’historique des messages
<img src="images/historique.png" alt="kafka-historique" width="600"/>
☕ Intégration de Kafka avec Spring Boot
📤 Publication de messages via REST

Un RestController Spring permet de publier des messages Kafka via un endpoint HTTP /publish, en précisant :

le nom du producer

le topic cible

<img src="images/RestKafka.png" alt="RestKafka" width="600"/>
📥 Consommation de messages avec Spring Kafka

Au lieu d’utiliser un consumer en ligne de commande, un consumer Kafka est implémenté côté Spring, ce qui permet :

une meilleure intégration applicative

un traitement métier automatisé

une gestion centralisée des messages

<img src="images/consumerSpring.png" alt="consumerSpring" width="600"/>
⏱️ Production automatique avec Supplier

Un Supplier Spring Cloud Stream est utilisé pour envoyer automatiquement un message Kafka toutes les secondes, ce qui est utile pour :

simuler un flux continu d’événements

tester le traitement temps réel

<img src="images/supplier.png" alt="supplierKafka" width="600"/>
📊 Analytics en temps réel (Kafka Streams + SSE)
🔎 Endpoint d’analytics temps réel

Une fonctionnalité avancée d’analyse temps réel a été implémentée à l’aide de Kafka Streams et des Server-Sent Events (SSE).

L’endpoint GET /analytics :

expose un flux continu de données

envoie automatiquement les mises à jour au client

Fonctionnement :

toutes les 1 seconde

interrogation du state store Kafka Streams count-store

récupération des données sur une fenêtre glissante des 5 dernières secondes

agrégation des résultats sous forme de Map<String, Long>

diffusion continue vers le client

Ce mécanisme permet une visualisation temps réel des événements sans rafraîchissement manuel.

<img src="images/tempreelback.png" alt="analyticsKafka" width="600"/>
🌐 Consommation côté Frontend

Côté frontend, l’API EventSource est utilisée pour se connecter à l’endpoint SSE /analytics et recevoir automatiquement les mises à jour en temps réel.

<img src="images/tempreelfront.png" alt="analyticsKafkaFront" width="600"/>