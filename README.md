# 🛡️ Projet SOC / SIEM – Suricata & Filebeat sur Docker


---

## 📌 Objectif du projet

Mettre en place une infrastructure de supervision de sécurité basée sur **Suricata** (IDS) et **Filebeat** (agent de collecte), avec une logique de pipeline JSON pour enrichir les logs. Le tout fonctionne dans un environnement **Dockerisé**.

---

## 🧱 Structure du projet

```
SOC-PROJET/
├── pipeline/
│   └── add_timestamp.json
│
├── suricata/
│   ├── logs/
│   │   ├── eve.json
│   │   ├── fast.log
│   │   ├── stats.log
│   │   └── suricata.log
│   │
│   └── rules/
│       └── custom.rules
│
│   └── suricata.yaml
│
├── docker-compose.yml
├── Dockerfile.filebeat
├── filebeat.yml
```

---

## 🧰 Technologies utilisées

| Composant          | Rôle                                                |
|--------------------|-----------------------------------------------------|
| **Suricata**       | IDS – Analyse du trafic réseau                      |
| **Filebeat**       | Agent de collecte des logs de Suricata              |
| **Pipeline JSON**  | Ajout de `@timestamp` pour l’indexation temporelle  |
| **Docker Compose** | Orchestration multi-conteneurs                      |

---

## ⚙️ Fonctionnement

- **Suricata** surveille le trafic et génère des logs dans `/logs`.
- **Filebeat** lit ces logs et applique un pipeline JSON.
- Les logs enrichis peuvent être envoyés vers Elasticsearch, stdout, etc.

---

## 🔧 Fichier Dockerfile personnalisé

```dockerfile
FROM docker.elastic.co/beats/filebeat:8.12.1

COPY filebeat.yml /usr/share/filebeat/filebeat.yml

USER root
RUN chmod 600 /usr/share/filebeat/filebeat.yml
```

🎯 Utilisé pour injecter une configuration personnalisée de Filebeat et gérer les permissions.

---

## 🧪 Scénarios de test

### 🔹 Scan Nmap depuis Kali
- Détection par Suricata (`ET SCAN Nmap`)
- Log généré dans `eve.json` et `fast.log`

### 🔹 Connexion SSH vers Ubuntu
- Port 22 surveillé
- Détection par Suricata, log visible dans les fichiers

---

## 📈 Résultat attendu

- Logs de sécurité détectés en temps réel
- Pipeline JSON opérationnel (`add_timestamp.json`)
- Envoi des événements enrichis vers la sortie souhaitée

---

## 🚀 Lancer l’infrastructure

```bash
docker-compose up -d
```

💡 Vérifie dans `suricata.yaml` que l’interface réseau est bien définie (ex : `eth0` ou `ens33`).

---

## 🚧 Améliorations possibles

- Intégration d’Elasticsearch + Kibana ou Grafana + Loki
- Ajout de règles Suricata personnalisées plus fines
- Simulation d’attaques avec Metasploit ou Hping3
- Intégration d’un VPN ou d’un bastion SSH

---

## 📚 Références

- [Suricata Docs](https://suricata.readthedocs.io/)
- [Filebeat Docs](https://www.elastic.co/guide/en/beats/filebeat/)
- [Docker Compose](https://docs.docker.com/compose/)
