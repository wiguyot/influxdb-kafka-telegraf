# TP — InfluxDB ↔ Telegraf ↔ Kafka

## Objectifs
- Monter un pipeline E2E : **Telegraf → InfluxDB → Kafka**.
- Comprendre la config `[[inputs.*]]` et `[[outputs.kafka]]`.
- Observer le flux côté Kafka (consumer CLI / **Kafdrop**).

## Prérequis
- Docker + Docker Compose, Git.
- Accès aux services :
  - InfluxDB : `http://<HOST>:<INFLUXDB_HTTP_PORT>`  <!-- TODO: remplace -->
  - Kafka broker : `<KAFKA_BROKER_HOST>:<KAFKA_BROKER_PORT>`  <!-- TODO -->
  - Kafdrop : `http://<HOST>:<KAFDROP_PORT>`  <!-- TODO -->
- (Option) Client Kafka (CLI) ou Kafdrop.

## Démarrage rapide
```bash
git clone <ce dépôt>
cd influxdb-kafka-telegraf
docker compose up -d
docker compose ps
```

## TP pas à pas
	1.	InfluxDB : créer/valider le bucket cible (UI ou API), récupérer INFLUX_TOKEN.
	2.	Telegraf : ouvrir telegraf.conf :
        • [[inputs.*]] (CPU/mem/net ou input Influx/HTTP).
        • [[outputs.kafka]] → broker PLAINTEXT://<KAFKA_BROKER_HOST>:<KAFKA_BROKER_PORT> et topic metrics.
	3.	Relancer Telegraf, vérifier les logs.
	4.	Kafka : créer le topic si besoin, consommer via CLI/Kafdrop.
	5.	Expérimenter : changer clé de partition et #partitions ; observer l’impact (débit/ordre intra-partition).


## Critères de réussite
	•	Messages visibles en temps réel dans Kafka/Kafdrop.
	•	Consumer lit sans erreur ; débit stable après changement de partitions.
	•	Mini rapport (10 lignes) expliquant la chaîne et la clé de partition choisie.
