TP4 : Configuration de base
---

### Challenge :

**Consignes :** 

Placez-vous dans le répertoire du troisième atelier pratique :

```
$ cd ~/formation-ansible/atelier-04
```
Voici les quatre machines virtuelles Ubuntu 22.04 de cet atelier :

| Machine virtuelle | Adresse IP        |
| :---------------- | :---------------- |
| control	        | 192.168.56.10     |
| target01	        | 192.168.56.20     |
| target02	        | 192.168.56.30     |
| target03	        | 192.168.56.40     |

Démarrez les VM :
```
$ vagrant up
```
Connectez-vous au Control Host :
```
$ vagrant ssh control
```

Réaliser les points suivants :
* Éditez ```/etc/hosts``` de manière à ce que les _Target Hosts_ soient joignables par leur nom d'hôte simple.
* Configurez l'authentification par clé SSH avec les trois _Target Hosts_.
* Installez Ansible.
* Envoyez un premier ```ping``` Ansible sans configuration.
* Créez un répertoire de projet ```~/monprojet```.
* Créez un fichier vide ```ansible.cfg``` dans ce répertoire.
* Vérifiez si ce fichier est bien pris en compte par Ansible.
* Spécifiez un inventaire nommé ```hosts```.
* Activez la journalisation dans ```~/journal/ansible.log```.
* Testez la journalisation.
* Créez un groupe ```[testlab]``` avec vos trois _Target Hosts_.
* Définissez explicitement l'utilisateur ```vagrant``` pour la connexion à vos cibles.
* Envoyez un ```ping``` Ansible vers le groupe de machines ```[all]```.
* Définissez l'élévation des droits pour l'utilisateur ```vagrant``` sur les _Target Hosts_.
* Affichez la première ligne du fichier ```/etc/shadow``` sur tous les _Target Hosts_.
* Quittez le _Control Host_ et supprimez toutes les VM de l'atelier.

---

#### Réponse : 

**1 - Éditez ```/etc/hosts``` de manière à ce que les _Target Hosts_ soient joignables par leur nom d'hôte simple :**

De la même manière que dans le TP précédent, je commence par me connecter au _Control Host_ :
```
$ cd atelier-06/
$ vagrant up
$ vagrant ssh control
```

Puis j'accède au fichier ```/etc/hosts``` pour y référencer mes cibles :
```
$ sudo nano /etc/hosts

192.168.56.20   target01
192.168.56.30   target02
192.168.56.40   target03
```

capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP4_configuration-de-base/screenshots/tp4_1_Ian-Bertin_Hugo-Crepin.png

**2 - Configurez l'authentification par clé SSH avec les trois _Target Hosts_ :**

Je commence par générer une clé SSH avec la configuration par défaut : 
```
$ ssh-heygen

Generating public/private rsa key pair.
Enter file in which to save the key (/home/vagrant/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/vagrant/.ssh/id_rsa
Your public key has been saved in /home/vagrant/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:sR0ctg1oFu1Bcf8gU9fgJgivYTUZsFYAtVyfJU0Rmsc vagrant@control
The key's randomart image is:
+---[RSA 3072]----+
|      .o*B%+ooB+o|
|       .+@+X @...|
|       oO.*.X E  |
|       o *.. * o |
|        S .     .|
|                 |
|                 |
|                 |
|                 |
+----[SHA256]-----+
```

Puis je configure l'authentification par clé des 3 _Target Hosts_ :

```
$ ssh-keyscan -t rsa target01 target02 target03 >> .ssh/known_hosts

# target01:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.13
# target02:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.13
# target03:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.13
```

capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP4_configuration-de-base/screenshots/tp4_2_Ian-Bertin_Hugo-Crepin.png

Et enfin, je distribue la clé publique sur mes Target Hosts en founrissant à chaque fois le mot de passe ```vagrant``` (Il faut le faire sur les trois, mais je n'en montre qu'un pour l'exemple) :

```
$ ssh-copy-id vagrant@target01

/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/vagrant/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
vagrant@target01's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'vagrant@target01'"
and check to make sure that only the key(s) you wanted were added.
```

capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP4_configuration-de-base/screenshots/tp4_3_Ian-Bertin_Hugo-Crepin.png

**3 - Installez Ansible :**

Je commencer par vérifier si ansible n'est pas déjà installé sur ma VM :

```
$ type ansible
-bash: type: ansible: not found
```

Je vérifie l'OS de ma VM :

```
$ cat /etc/os-release

PRETTY_NAME="Ubuntu 22.04.5 LTS"
NAME="Ubuntu"
VERSION_ID="22.04"
VERSION="22.04.5 LTS (Jammy Jellyfish)"
VERSION_CODENAME=jammy
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=jammy
```

capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP4_configuration-de-base/screenshots/tp4_4_Ian-Bertin_Hugo-Crepin.png

Je met donc à jour la liste des paquets puis j'installe ansible :

```
$ sudo apt update
$ sudo apt install -y ansible
```

Enfin, je vérifie l'installation :

```
$ type ansible
ansible is /usr/bin/ansible
```

capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP4_configuration-de-base/screenshots/tp4_5_Ian-Bertin_Hugo-Crepin.png

**4 - Envoyez un premier ```ping``` Ansible sans configuration :**

```
ansible all -m ping
```
Le ping échoue

**5 - Créez un répertoire de projet ```~/monprojet``` :**

```
mkdir monprojet
```

**6 - Créez un fichier vide ```ansible.cfg``` dans ce répertoire :**

```
touch ansible.cfg
```

**7 - Vérifiez si ce fichier est bien pris en compte par Ansible :**

```
$ ansible --version | head -n 2
ansible 2.10.8
  config file = /home/vagrant/monprojet/ansible.cfg
```

capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP4_configuration-de-base/screenshots/tp4_6_Ian-Bertin_Hugo-Crepin.png

**8 - Spécifiez un inventaire nommé ```hosts``` :**

```
$ nano hosts
$ cat hosts

[testing]
target01
target02
target03
```

capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP4_configuration-de-base/screenshots/tp4_7_Ian-Bertin_Hugo-Crepin.png

**9 - Activez la journalisation dans ```~/journal/ansible.log``` :**

On créer le fichier de destination :

```
$ mkdir ~/journal
$ touch ~/journal/ansible.log
```

On ajoute la destination au fichier de configuration de ansible :

```
$ nano ansible.cfg

[defaults]
inventory = ./hosts
log_path = ~/journal/ansible.log
```

**10 - Testez la journalisation :**

On ping pour tester que la journalisation fonctionne bien :

```
$ ansible all -m ping
```

Puis on vérifie :
```
$ cat ~/journal/ansible.log

2025-11-10 17:01:27,896 p=3228 u=vagrant n=ansible | target02 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
2025-11-10 17:01:27,900 p=3228 u=vagrant n=ansible | target01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
2025-11-10 17:01:27,920 p=3228 u=vagrant n=ansible | target03 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP4_configuration-de-base/screenshots/tp4_8_Ian-Bertin_Hugo-Crepin.png

**11 & 12 - Créez un groupe ```[testlab]``` avec vos trois _Target Hosts_, Définissez explicitement l'utilisateur ```vagrant``` pour la connexion à vos cibles :**

On modifie notre fichier ```hosts``` :

```
$ nano hosts

[testlab]
target01
target02
target03

[testlab:vars]
ansible_python_interpreter=/usr/bin/python3
ansible_user=vagrant
```

capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP4_configuration-de-base/screenshots/tp4_9_Ian-Bertin_Hugo-Crepin.png

**13 - Envoyez un ```ping``` Ansible vers le groupe de machines ```[all]``` :**

```
$ ansible all -m ping

target01 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
target03 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
target02 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

**14 -  Définissez l'élévation des droits pour l'utilisateur ```vagrant``` sur les _Target Hosts_ :**

Je rajoute simplement ```ansible_become=true``` dans mon fichier ```hosts``` :

```
$ nano hosts

[testlab]
target01
target02
target03

[testlab:vars]
ansible_python_interpreter=/usr/bin/python3
ansible_user=vagrant

ansible_become=true
```

capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP4_configuration-de-base/screenshots/tp4_10_Ian-Bertin_Hugo-Crepin.png

**15 - Affichez la première ligne du fichier ```/etc/shadow``` sur tous les _Target Hosts_ :**

```
$ ansible all -a "head -n 1 /etc/shadow"

target03 | CHANGED | rc=0 >>
root:*:19977:0:99999:7:::
target02 | CHANGED | rc=0 >>
root:*:19977:0:99999:7:::
target01 | CHANGED | rc=0 >>
root:*:19977:0:99999:7:::
```

capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP4_configuration-de-base/screenshots/tp4_11_Ian-Bertin_Hugo-Crepin.png

**16 - Quittez le _Control Host_ et supprimez toutes les VM de l'atelier :**

```
$ exit
$ vagrant destroy -f
```

capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP4_configuration-de-base/screenshots/tp4_12_Ian-Bertin_Hugo-Crepin.png