Objectif : un script envoie dans InfluxDB, un check est réalisé, la notification rule est activée pour finir par appeler un endpoint qui appel un producteur kafka


Etat des 


<img src="docs/assets/check-push-to-kafka.png"
     alt="check push to kafka"
     width="400">

<img src="docs/assets/notification-endpoint.png"
     alt="notification endpoint"
     width="400">

<img src="docs/assets/notification-rule.png"
     alt="notification rule"
     width="400">


```pgsql
+----------------+
|  inject        |  ← Génère données météo simulées
|  (inject.py)   |
+-------+--------+
        |
        | InfluxDB Client API
        v
+----------------+
|  influxdb      |  ← Stocke les mesures (ville, température, timestamp)
+-------+--------+
        |
        | Flux Query API
        v
+----------------+
|  producer      |  ← Lit les nouvelles données dans InfluxDB
|  (producer.py) |     et les envoie vers Kafka (clé = ville)
+-------+--------+
        |
        | Kafka Protocol
        v
+----------------+
|  kafka[1..3]   |  ← Cluster KRaft, topic `weather` (4 partitions, RF=3)
+-------+--------+
        |
        | HTTP REST
        v
+----------------+
|  kafdrop       |  ← UI pour inspecter le cluster, les topics, les messages
+----------------+
```