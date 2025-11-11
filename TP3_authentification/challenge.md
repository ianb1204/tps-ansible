TP3 : Authentification
---

### Challenge :

**Consignes :** 

Placez-vous dans le répertoire du troisième atelier pratique :

```
$ cd ~/formation-ansible/atelier-03
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

Ansible est déjà installé sur cette machine :
```
$ type ansible
ansible is /usr/bin/ansible
```

Faites le nécessaire pour réussir un ping Ansible comme ceci :

```
$ ansible all -i target01,target02,target03 -m ping
target03 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target02 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

---

#### Réponses : 

1 - Je commence tout d'abord par me connecter au _Control Host_ :
```
$ cd atelier-03/
$ vagrant up
$ vagrant ssh control
```

capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP3_authentification/screenshots/tp3_1_Ian-Bertin_Hugo-Crepin.png

2 - J'ajoute ensuite les _Target Hosts_ à la liste des résolutions locales de ma VM :

```
$ sudo nano /etc/hosts

192.168.56.20   target01
192.168.56.30   target02
192.168.56.40   target03
```

capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP3_authentification/screenshots/tp3_2_Ian-Bertin_Hugo-Crepin.png

3 - Je teste la connectivité de base de mes VMs

```
$ for HOST in target01 target02 target03; do ping -c 1 -q $HOST; done

PING target01 (192.168.56.20) 56(84) bytes of data.

--- target01 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.215/0.215/0.215/0.000 ms
PING target02 (192.168.56.30) 56(84) bytes of data.

--- target02 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.216/0.216/0.216/0.000 ms
PING target03 (192.168.56.40) 56(84) bytes of data.

--- target03 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.193/0.193/0.193/0.000 ms
```

capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP3_authentification/screenshots/tp3_2_2_Ian-Bertin_Hugo-Crepin.png

4 - Je collecte les clés ssh publiques des _Target Hosts_ :

```
$ ssh-keyscan -t rsa target01 target02 target03 >> .ssh/known_hosts

# target01:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.13
# target02:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.13
# target03:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.13
```

capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP3_authentification/screenshots/tp3_3_Ian-Bertin_Hugo-Crepin.png

5 - Je génère une paire de clés SSH en acceptant toutes les options par défaut

```
$ ssh-keygen
```

capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP3_authentification/screenshots/tp3_4_Ian-Bertin_Hugo-Crepin.png

6 - Je distribue la clé publique sur mes Target Hosts en founrissant à chaque fois le mot de passe ```vagrant``` :

```
$ ssh-copy-id vagrant@target01
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/vagrant/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
vagrant@target01's password:

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'vagrant@target01'"
and check to make sure that only the key(s) you wanted were added.

$ ssh-copy-id vagrant@target02
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/vagrant/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
vagrant@target02's password:

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'vagrant@target02'"
and check to make sure that only the key(s) you wanted were added.

$ ssh-copy-id vagrant@target03
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/vagrant/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
vagrant@target03's password:

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'vagrant@target03'"
and check to make sure that only the key(s) you wanted were added.
```

capture d'écran (seulement un exemple) : https://github.com/ianb1204/tps-ansible/blob/main/TP3_authentification/screenshots/tp3_5_Ian-Bertin_Hugo-Crepin.png

7 - J'effectue le ping demandé dans la consigne :

```
$ ansible all -i target01,target02,target03 -m ping

target03 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target02 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP3_authentification/screenshots/tp3_6_Ian-Bertin_Hugo-Crepin.png