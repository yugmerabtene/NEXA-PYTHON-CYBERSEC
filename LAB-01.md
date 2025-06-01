## LAB-01
* Lire et parser des fichiers de logs (Apache, Nginx ou SSH).
* Détecter des comportements suspects (ex. brute-force, scanning, accès interdits).
* Générer un rapport simple.

---

##  **LAB : Analyse de logs serveur pour détection d’activités suspectes en Python**

###  Objectifs pédagogiques

* Lire et analyser des fichiers de logs système ou web.
* Détecter des signes d’attaque courants (brute-force, accès 403, scans).
* Comprendre les patterns d’expression régulière (regex).
* Générer un rapport automatisé.

---

## 🛠️ Prérequis

* Python 3.8+
* Modules : `re`, `datetime`, `collections`
* Fichier de logs Apache ou SSH (ex. `/var/log/auth.log`, `/var/log/apache2/access.log`)

---

## 📁 Structure du projet

```
log-analyzer/
├── logs/
│   └── access.log              ← log à analyser
├── analyzer.py                 ← script principal
├── utils.py                    ← fonctions de support
└── report.txt                  ← rapport généré
```

---
