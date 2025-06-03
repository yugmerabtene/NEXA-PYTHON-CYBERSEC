# NEXA-PYTHON-CYBERSEC

**NEXA-PYTHON-CYBERSEC** est une série de laboratoires pratiques en cybersécurité développés en Python.
Chaque lab vous place dans la peau d’un **analyste**, **défenseur (Blue Team)** ou **attaquant (Red Team)** dans des scénarios concrets.
L’objectif est d’apprendre à utiliser Python dans des contextes de **cybersécurité offensive, défensive ou d’analyse forensique**.

---

## Objectifs pédagogiques

* Mettre en pratique Python dans des cas réels de cybersécurité
* Renforcer les compétences en réseau, forensic, Blue Team et Red Team
* Automatiser des tâches de détection, d’analyse ou d’attaque contrôlée
* Maîtriser des modules essentiels : `scapy`, `socket`, `hashlib`, `os`, `re`, etc.

---

## Liste des Labs

| LAB | Nom du projet           | Objectif principal                                        | Niveau        |
| --- | ----------------------- | --------------------------------------------------------- | ------------- |
| 01  | Analyse de logs système | Détecter des anomalies dans des logs SSH ou UFW           | Débutant      |
| 02  | Surveillance DNS        | Sniffer le trafic DNS et identifier des domaines suspects | Intermédiaire |
| 03  | À venir...              |                                                           | -             |

---

## Structure du projet

```
NEXA-PYTHON-CYBERSEC/
│
├── LAB-01-Analyse-Logs/
│   ├── lab01.py
│   ├── auth.log
│   ├── ufw.log
│   └── report.txt
│
├── LAB-02-DNS-Monitoring/
│   ├── lab02.py
│   ├── blacklist.txt
│   ├── dns_alerts.log
│   ├── summary_report.txt
│   └── dns_simulator.py (optionnel)
│
├── LAB-03-.../
│   └── ...
│
└── README.md
```

Chaque LAB contient :

* Un script principal (`labXX.py`)
* Des fichiers de ressources (`*.log`, `*.txt`, `*.json`, etc.)
* Un README spécifique (si nécessaire)
* Un rapport généré (résumé ou log d’analyse)

---

## Prérequis

* Python 3.8 ou supérieur
* Système Linux (ou WSL sous Windows)
* Droits sudo (nécessaires pour certaines captures réseau)
* Modules recommandés : `scapy`, `colorama`, `argparse`, etc.

Installation des dépendances :

```
pip install -r requirements.txt
```

Le fichier `requirements.txt` sera mis à jour à mesure de l’ajout de nouveaux labs.

---

## Conseils d’utilisation

* Exécute chaque lab depuis son dossier avec :
  `sudo python3 labXX.py`
* Vérifie que tu as les droits nécessaires pour le sniffing réseau
* Utilise les simulateurs fournis pour générer des événements de test
