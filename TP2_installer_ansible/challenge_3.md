TP3 : Installer Ansible
---

### Challenge n°3 :

**Consignes :** 

Lancez une VM Rocky Linux et installez Ansible en utilisant PIP et Virtualenv. 

---

#### Réponses : 

**1 - Je commence par monter la VM Rocky Linux :**
```
$ cd atelier-01
$ vagrant up rocky
```

capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP2_installer_ansible/screenshots/tp2_3_1_Ian-Bertin_Hugo-Crepin.png

**2 - Je me connecte à la VM montée :**
```
$ vagrant ssh rocky
```
capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP2_installer_ansible/screenshots/tp2_3_2_Ian-Bertin_Hugo-Crepin.png

**3 - J'install venv :**

J'essaie d'abord de trouver le paquet venv dans le gestionnaire dnf.
```
$ sudo dnf install venv
```
On ne trouve rien.
```
No match for argument: venv
Error : Unable to find a match : venv
```
capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP2_installer_ansible/screenshots/tp2_3_3_Ian-Bertin_Hugo-Crepin.png

Étant donné qu'on ne peut pas y accéder via le gestionnaire dnf, on va passer par pip pour utiliser le paquet venv :
```
$ sudo dnf install python3-pip
```

capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP2_installer_ansible/screenshots/tp2_3_4_Ian-Bertin_Hugo-Crepin.png

**4 - Création et activation de l'environnement virtuel :**

```
[vagrant@rocky ~]$ python3 -m venv ~/.venv/challenge3
[vagrant@rocky ~]$ source ~/.venv/challenge3
(challenge 3) [vagrant@rocky ~]$
```

On vérifie si ansible est déjà dans l'environnnement virtuel :
```
(challenge 3) [vagrant@rocky ~]$ ansible --version
-bash: ansible: command not found
```

capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP2_installer_ansible/screenshots/tp2_3_5_Ian-Bertin_Hugo-Crepin.png

**4 - Installation de Ansible :**

On commence par mettre à jour pip dans l'environnement virtuel :
```
$ pip install --upgrade pip
```

Puis on y installe asible :
```
$ pip install ansible
$ ansible --version
```

capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP2_installer_ansible/screenshots/tp2_3_6_Ian-Bertin_Hugo-Crepin.png