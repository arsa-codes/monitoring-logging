# 🎯 Supervision & Centralisation des Logs

## 🎯 Objectif
Mettre en place une stack de monitoring complète pour assurer la disponibilité et la performance de l'infrastructure.
Le projet couvre deux volets :
1. **Métriques (Time-Series)** : CPU, RAM, Disque (via Prometheus/Grafana).
2. **Logs (Analyse)** : Centralisation des journaux système (via ELK/Logstash).

## 🛠️ Outils
- **Prometheus** : Base de données temporelle pour stocker les métriques.
- **Grafana** : Tableaux de bord visuels (Dashboards).
- **Node Exporter** : Agent installé sur les serveurs pour exposer les métriques matérielles.
- **ELK Stack** : Configuration prête pour Logstash (ingestion), Elasticsearch (stockage) et Kibana (recherche).

## 🧪 Tests / Validation
Pour valider le fonctionnement de la stack :

### 1. Vérification des métriques (Prometheus)
Accéder à `http://localhost:9090/targets`.
*Résultat attendu* : Les endpoints `prometheus` et `node-exporter` doivent être en état **UP**.

### 2. Visualisation (Grafana)
Accéder à `http://localhost:3000` (admin/admin).
*Action* : Importer un Dashboard (ID 1860 pour Node Exporter).
*Résultat attendu* : Les graphiques CPU/RAM s'affichent en temps réel.

### 3. Parsing des Logs (Logstash)
Le fichier `elk/logstash/pipeline/logstash.conf` contient un filtre **Grok** validé pour parser les logs système standards (Syslog RFC 3164).

## 🔐 Sécurité
Mesures de sécurité implémentées dans une prod réelle :
- **Authentification** : Activation du login/password sur Prometheus (via Reverse Proxy Nginx) et Grafana.
- **TLS/SSL** : Chiffrement des flux entre les agents (Beats/Exporters) et le serveur central.
- **Firewall** :
  - Port 9090 (Prometheus) : Accès restreint au réseau de management.
  - Port 3000 (Grafana) : Accessible via VPN ou IP whitelistée.

## 📸 Architecture
`Server (Node Exporter) -> Pull (Prometheus) -> Visualize (Grafana)`
`Server (Filebeat) -> Push (Logstash) -> Store (Elasticsearch)`
