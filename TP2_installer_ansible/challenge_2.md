TP2 : Installer Ansible
---

### Challenge n°2 :

**Consignes :** 
Répétez le challenge précédent en configurant un dépôt PPA (Personal Package Archive) pour Ansible :
```
$ sudo apt-add-repository ppa:ansible/ansible
```

Notez la version fournie par ce dépôt tiers et comparez avec la version officielle du challenge précédent.

---

#### Réponses : 

**1 - Je commence par monter la VM Ubuntu du challenge n°1 :**
* ```$ cd atelier-01```
* ```$ vagrant up ubuntu```
* ```$ vagrant ssh ubuntu```
* ```$ sudo apt update```

**2 - Je configure le PPA pour ansible :**

Réponse : ```$ sudo apt-add-repository ppa:ansible/ansible```

capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP2_installer_ansible/screenshots/tp2_2_1_Ian-Bertin_Hugo-Crepin.png

**3 - J'installe ansible :**

Réponse : ```$ sudo apt install -y ansible```

capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP2_installer_ansible/screenshots/tp2_2_2_Ian-Bertin_Hugo-Crepin.png

**4 - Je vérifie l'installation :**

Réponse : ```$ ansible --version```

capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP2_installer_ansible/screenshots/tp2_2_3_Ian-Bertin_Hugo-Crepin.png

**5 - Je note la version du paquet installé :**

Réponse : 

```
ansible [core 2.17.14]
  config file = /etc/ansible/ansible.cfg
  configured module search path = ['/home/vagrant/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  ansible collection location = /home/vagrant/.ansible/collections:/usr/share/ansible/collections
  executable location = /usr/bin/ansible
  python version = 3.10.12 (main, Aug 15 2025, 14:32:43) [GCC 11.4.0] (/usr/bin/python3)
  jinja version = 3.0.3
  libyaml = True
```

capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP2_installer_ansible/screenshots/tp2_2_3_Ian-Bertin_Hugo-Crepin.png

**6 - Je compare avec la version installée dans le challenge n°1 :**

Version du challenge n°1 : ```ansible 2.10.8```

Version du challenge n°2 : ```ansible [core 2.17.14]```

La version installée à l'aide du dépôt PPA configuré dans le challenge n°2 est plus récente que celle du challenge n°1.
