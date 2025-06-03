# NEXA-PYTHON-CYBERSEC

NEXA-PYTHON-CYBERSEC est un ensemble de laboratoires pratiques orientés cybersécurité, développés en Python.
Chaque LAB vous met dans la peau d’un analyste, d’un défenseur ou d’un attaquant sur des cas concrets, pour apprendre à utiliser Python dans un contexte de sécurité offensive, défensive ou d'analyse.

---

## Objectifs pédagogiques

* Appliquer Python à des scénarios de cybersécurité réels
* Renforcer les compétences en réseau, forensic, Blue Team et Red Team
* Automatiser des tâches de détection, d’analyse ou d’attaque contrôlée
* Maîtriser des outils comme `scapy`, `socket`, `hashlib`, `os`, `re`, etc.

---

## Liste des Labs

| LAB | Nom du projet           | Objectif principal                                      | Niveau        |
| --- | ----------------------- | ------------------------------------------------------- | ------------- |
| 01  | Analyse de logs système | Détecter des anomalies dans les logs SSH ou UFW         | Débutant      |
| 02  | Surveillance DNS        | Sniffer le trafic DNS et détecter des domaines suspects | Intermédiaire |
| 03  | À venir...              |                                                         | -             |

Chaque LAB est contenu dans un dossier dédié avec :

* Le script principal en Python (`labXX.py`)
* Un ou plusieurs fichiers de ressources (`*.log`, `*.txt`, `*.json`)
* Un README spécifique si besoin
* Un rapport généré (log ou résumé)

---

## Structure du projet

NEXA-PYTHON-CYBERSEC/
│
├── LAB-01-Analyse-Logs/
│   ├── lab01.py
│   ├── auth.log
│   ├── ufw\.log
│   └── report.txt
│
├── LAB-02-DNS-Monitoring/
│   ├── lab02.py
│   ├── blacklist.txt
│   ├── dns\_alerts.log
│   ├── summary\_report.txt
│   └── dns\_simulator.py (optionnel)
│
├── LAB-03-.../
│   └── ...
│
└── README.md

---

## Prérequis

* Python 3.8 ou supérieur
* Linux (ou WSL sous Windows pour les labs réseau)
* Droits sudo pour certaines captures réseau
* Modules recommandés : `scapy`, `colorama`, `argparse`, etc.

Installation des dépendances :

```
pip install -r requirements.txt
```

Un fichier `requirements.txt` sera fourni au fil de l’ajout des LABs.

---

## Conseils d’utilisation

* Lance chaque lab depuis son dossier avec `sudo python3 labXX.py`
* Vérifie que tu as les permissions nécessaires si tu fais du sniffing réseau
* Utilise les simulateurs inclus pour générer des événements artificiels de test

