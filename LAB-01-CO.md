## CORRECTION DU LAB-01

## Structure du projet

```
log-analyzer/
├── logs/
│   └── access.log              ← log à analyser
├── analyzer.py                 ← script principal
├── utils.py                    ← fonctions de support
└── report.txt                  ← rapport généré
```

## 📜 Étape 1 – Chargement et lecture du fichier de log

```python
# analyzer.py
import re
from utils import load_log_file, save_report
from collections import defaultdict

log_path = "logs/access.log"
lines = load_log_file(log_path)

suspicious_ips = defaultdict(int)
denied_access = []
scan_detected = []

for line in lines:
    # Exemple log Apache : 192.168.1.1 - - [01/Jun/2025:10:15:32 +0200] "GET /admin HTTP/1.1" 403 721
    match = re.search(r'(?P<ip>\d+\.\d+\.\d+\.\d+).*\[(?P<datetime>[^\]]+)\] "(?P<method>GET|POST) (?P<url>.*?) HTTP/1.[01]" (?P<status>\d+)', line)
    if match:
        ip = match.group('ip')
        url = match.group('url')
        status = int(match.group('status'))

        if status == 403:
            denied_access.append((ip, url))
            suspicious_ips[ip] += 1

        if "wp-login" in url or "phpmyadmin" in url:
            scan_detected.append((ip, url))
            suspicious_ips[ip] += 1

# Génération du rapport
report_lines = []

report_lines.append(" Analyse de logs : Résumé")
report_lines.append(f"Nombre total de lignes : {len(lines)}")
report_lines.append(f"Nombre d’accès interdits (403) : {len(denied_access)}")
report_lines.append(f"Scans détectés (URLs sensibles) : {len(scan_detected)}")

report_lines.append("\n IPs suspectes (plus de 3 événements)")
for ip, count in suspicious_ips.items():
    if count > 3:
        report_lines.append(f" - {ip} : {count} tentatives")

save_report("report.txt", report_lines)
print(" Rapport généré dans 'report.txt'")
```

---

##  Étape 2 – Fonctions utilitaires (`utils.py`)

```python
# utils.py
def load_log_file(path):
    with open(path, 'r', encoding='utf-8', errors='ignore') as f:
        return f.readlines()

def save_report(path, lines):
    with open(path, 'w', encoding='utf-8') as f:
        for line in lines:
            f.write(line + "\n")
```

---

##  Étape 3 – Exécution du LAB

1. Copiez un fichier de log réel dans `logs/access.log`.
2. Lancez le script avec :

```bash
python3 analyzer.py
```

3. Le fichier `report.txt` contiendra les résultats de l’analyse.

---

##  Bonus – Extensions possibles

* Gérer les logs SSH (`/var/log/auth.log`) avec détection de `Failed password`.
* Visualiser les IPs avec `matplotlib` ou `folium` (localisation géographique).
* Exporter en JSON/CSV pour dashboard.
* Détection d’anomalies horaires (ex. connexions à 3h du matin).
