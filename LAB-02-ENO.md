## LAB-02 – Surveillance DNS et détection de domaines suspects

### Objectif du laboratoire

Ce laboratoire vous permet de :

* Capturer des requêtes DNS en temps réel sur un réseau local.
* Analyser les noms de domaine demandés.
* Identifier des activités suspectes via une liste noire et des règles comportementales.
* Générer des alertes et logs de sécurité.
* Simuler des actions de Blue Team (défense) et de Red Team (attaque via DNS).

---

### Contexte

Un administrateur réseau soupçonne une machine de contacter des serveurs malveillants par le biais de requêtes DNS. Il vous confie la mission de développer un outil de détection basé sur Python capable de :

* Capturer les paquets DNS en temps réel.
* Comparer les domaines demandés avec une liste noire.
* Identifier des requêtes DNS anormales ou suspectes.

---

### Ressources fournies

* `blacklist.txt` : une liste de domaines malveillants connus.
* `dns_simulator.py` : simulateur de trafic DNS mixte (bénin + malveillant).
* Fichier partiellement rempli `lab02.py` à compléter.
* Un réseau local Linux de test (machine virtuelle ou réelle).

---

### Travaux à réaliser

#### 1. Capture de trafic DNS (Live Sniffing)

* Utilisez la bibliothèque `scapy` pour écouter l’interface réseau (`eth0`, `wlan0`, etc.).
* Filtrez uniquement les requêtes DNS de type A (IPv4).
* Pour chaque requête, affichez :

  * L’IP source
  * Le nom de domaine demandé
  * L’heure

#### 2. Détection d’anomalies

* Si le domaine est dans la liste noire, générez une alerte niveau *critique*.
* Si le domaine a un comportement suspect :

  * Domaine très long (plus de 50 caractères)
  * Utilisation de TLD rares : `.top`, `.xyz`, `.ru`, etc.
  * Pattern aléatoire (exemple : `a1b2c3d4e5f6.xyz`)
  * Fréquence de requêtes élevée par IP (plus de 10 par minute)

Attribuez un score de suspicion à chaque domaine (entre 0 et 100).

#### 3. Journalisation & Rapport

* Enregistrez les alertes dans un fichier `dns_alerts.log`.
  Format recommandé :

  ```
  [2025-06-03 11:28:42] ALERT - IP: 192.168.1.34 - Domain: suspiciousdomain.ru - Score: 85 - Status: CRITICAL
  ```

* À la fin du programme ou sur interruption (CTRL+C), générez un fichier `summary_report.txt` contenant :

  * La liste des IPs suspectes
  * Les domaines contactés
  * Le score maximal observé
  * Une recommandation (bloquer, surveiller, ignorer)

#### 4. Défense active (facultatif mais recommandé)

* Si un domaine est critique (score supérieur ou égal à 80), ajoutez une règle `iptables` pour bloquer l’IP source localement :

  ```bash
  iptables -A INPUT -s <IP> -j DROP
  ```

---

### Contraintes techniques

* Le script doit fonctionner uniquement sous Linux.
* L'utilisation de la bibliothèque `scapy` est obligatoire.
* Vous pouvez utiliser : `re`, `datetime`, `os`, `argparse`, `signal`, etc.
* Le code doit être clair, modulaire, commenté et bien structuré.

---

### Bonus (niveau avancé)

* Implémenter un mode graphique ou web avec Flask pour afficher les alertes en direct.
* Simuler une attaque d’exfiltration DNS en encodant des données dans des sous-domaines.
* Ajouter une liste blanche de domaines sûrs.
* Générer des graphiques statistiques avec `matplotlib` ou `plotly`.
