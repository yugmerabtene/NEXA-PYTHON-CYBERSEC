# **LAB-02 – Surveillance DNS et Détection de Domaines Suspects**

**Langage** : Python
**Niveau** : intermédiaire
**Système requis** : Linux (ou WSL sous Windows)
**Module externe** : `scapy`

---

## 1. Objectif du Lab

Créer un outil de cybersécurité capable de :

* Surveiller les requêtes DNS sur un réseau local.
* Analyser les domaines demandés.
* Détecter les domaines malveillants via une blacklist.
* Identifier des comportements anormaux via des règles.
* Générer des rapports et logs.

---

## 2. Installation requise

Avant tout, installe les dépendances nécessaires :

```bash
pip install scapy
```

Puis exécute le script avec des privilèges administrateur pour pouvoir sniffer les paquets :

```bash
sudo python3 lab02.py
```

---

## 3. Code source expliqué

### Importation des modules

```python
from scapy.all import sniff, DNSQR, IP
from datetime import datetime
import re
import signal
import sys
```

* `scapy` permet d'intercepter les paquets réseau.
* `datetime` sert à horodater les événements.
* `re` pour les expressions régulières.
* `signal` permet de capturer CTRL+C pour un arrêt propre.
* `sys` permet de sortir proprement du script.

---

### Définition des règles

```python
BLACKLIST = {
    "malicious-domain.com",
    "suspiciousdomain.ru",
    "data-leak.xyz"
}

SUSPICIOUS_TLDS = {".ru", ".xyz", ".top"}
```

* `BLACKLIST` contient des domaines connus comme malveillants.
* `SUSPICIOUS_TLDS` contient des TLD (extensions de domaine) souvent utilisés pour des attaques.

---

### Variables globales

```python
alerts = []
query_count = {}
```

* `alerts` : liste des événements suspectés.
* `query_count` : permet de suivre la fréquence des requêtes par IP par minute.

---

### Fonction de scoring

```python
def score_domain(domain, src_ip):
    score = 0
    if domain in BLACKLIST:
        score += 80
    if len(domain) > 50:
        score += 10
    if any(domain.endswith(tld) for tld in SUSPICIOUS_TLDS):
        score += 5
    if re.match(r"^[a-z0-9]{12,}\.(ru|xyz|top)$", domain):
        score += 15

    now = datetime.now()
    key = (src_ip, now.strftime("%Y-%m-%d %H:%M"))
    query_count[key] = query_count.get(key, 0) + 1
    if query_count[key] > 10:
        score += 10

    return min(score, 100)
```

Cette fonction attribue un **score de suspicion** en fonction de critères :

* Présence dans la blacklist (critique),
* Longueur du domaine (obfuscation probable),
* TLDs exotiques,
* Fréquence élevée de requêtes (bruit réseau).

---

###  Traitement d’un paquet DNS

```python
def process_packet(packet):
    if packet.haslayer(DNSQR) and packet.haslayer(IP):
        domain = packet[DNSQR].qname.decode().strip(".")
        src_ip = packet[IP].src
        timestamp = datetime.now().strftime("%Y-%m-%d %H:%M:%S")

        score = score_domain(domain, src_ip)
        if score > 0:
            status = "INFO"
            if score >= 80:
                status = "CRITICAL"
            elif score >= 50:
                status = "WARNING"

            alert = f"[{timestamp}] ALERT - IP: {src_ip} - Domain: {domain} - Score: {score} - Status: {status}"
            alerts.append(alert)
            print(alert)
```

À chaque paquet capturé, on :

1. Extrait le nom de domaine demandé.
2. Applique le scoring.
3. Affiche une alerte si suspicion.

---

### Enregistrement des rapports

```python
def save_reports():
    with open("dns_alerts.log", "w") as f_log:
        for alert in alerts:
            f_log.write(alert + "\n")

    unique_ips = set()
    domains_contacted = set()
    max_score = 0

    for alert in alerts:
        parts = alert.split(" - ")
        ip = parts[1].split(": ")[1]
        domain = parts[2].split(": ")[1]
        score = int(parts[3].split(": ")[1])
        unique_ips.add(ip)
        domains_contacted.add(domain)
        max_score = max(max_score, score)

    with open("summary_report.txt", "w") as f_summary:
        f_summary.write("===== Résumé du LAB-02 - Analyse DNS =====\n")
        f_summary.write(f"IPs suspectes : {', '.join(unique_ips)}\n")
        f_summary.write(f"Domaines contactés : {', '.join(domains_contacted)}\n")
        f_summary.write(f"Score de suspicion maximal : {max_score}\n")
        if max_score >= 80:
            f_summary.write("Recommandation : Blocage immédiat de l'IP source\n")
        elif max_score >= 50:
            f_summary.write("Recommandation : Surveillance accrue\n")
        else:
            f_summary.write("Recommandation : Aucune action urgente\n")
```

Cette fonction crée :

* Un **log d’alertes DNS**.
* Un **rapport de synthèse final** avec des recommandations.

---

### Gestion d’interruption (CTRL+C)

```python
def signal_handler(sig, frame):
    print("\nArrêt détecté. Génération des rapports...")
    save_reports()
    sys.exit(0)
```

Permet de générer les fichiers à la fin de l’analyse même en cas d’arrêt manuel.

---

### Main

```python
if __name__ == "__main__":
    signal.signal(signal.SIGINT, signal_handler)
    print("Surveillance DNS en cours... (CTRL+C pour arrêter)")
    sniff(filter="udp port 53", prn=process_packet)
```

Active la capture réseau avec `sniff()` sur le port 53 (DNS). Appelle `process_packet` à chaque paquet.

---

## 4. Résultat attendu

Exécution du script :

```bash
Surveillance DNS en cours... (CTRL+C pour arrêter)
[2025-06-03 11:34:20] ALERT - IP: 192.168.1.45 - Domain: suspiciousdomain.ru - Score: 85 - Status: CRITICAL
[2025-06-03 11:34:22] ALERT - IP: 192.168.1.45 - Domain: b6c2e3f8a4d7.xyz - Score: 70 - Status: WARNING
```

Fichiers générés :

* `dns_alerts.log` : contient toutes les alertes.
* `summary_report.txt` : rapport de sécurité final.

---

## 5. Bonus (facultatif)

1. **Bloquer l’IP source** automatiquement :

```bash
iptables -A INPUT -s 192.168.1.45 -j DROP
```

2. **Afficher les alertes dans une page Web Flask**.

3. **Utiliser VirusTotal API** pour enrichir la détection.
