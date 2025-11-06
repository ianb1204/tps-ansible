TP2 : Installer Ansible
---

### Challenge n°1 :

**Consignes :** 
* Démarrez la VM ubuntu depuis le répertoire atelier-01.
* Connectez-vous à cette VM.
* Rafraîchissez les informations sur les paquets.
* Recherchez le paquet ansible avec les options qui vont bien.
* Installez le paquet officiel fourni par la distribution.
* Vérifiez si l'installation s'est bien déroulée.
* Notez la version d'Ansible.
* Déconnectez-vous et supprimez la VM.

---

#### Réponses : 

**1 - Démarrez la VM ubuntu depuis le répertoire atelier-01 :**

Réponses :
* ```cd atelier-01```
* ```vagrant up ubuntu```

capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP2_installer_ansible/tp2_1_1_Ian-Bertin_Hugo-Crepin.md

**2 - Connectez-vous à cette VM :**

Réponse : ```vagrant ssh ubuntu```

capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP2_installer_ansible/tp2_1_2_Ian-Bertin_Hugo-Crepin.md

**3 - Rafraîchissez les informations sur les paquets :**

Réponse : ```sudo apt update```

capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP2_installer_ansible/tp2_1_3_Ian-Bertin_Hugo-Crepin.md

**4 - Recherchez le paquet ansible avec les options qui vont bien :**

Réponse : ```apt-cache search --names-only ansible```

capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP2_installer_ansible/tp2_1_4_Ian-Bertin_Hugo-Crepin.md

**5 - Installez le paquet officiel fourni par la distribution :**

Réponse : ```sudo apt install -y ansible```

capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP2_installer_ansible/tp2_1_5_Ian-Bertin_Hugo-Crepin.md

**6 - Vérifiez si l'installation s'est bien déroulée :**

Réponse : ```ansible --version```

capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP2_installer_ansible/tp2_1_6_Ian-Bertin_Hugo-Crepin.md

**7 - Notez la version d'Ansible :**

Réponse : 
```
ansible 2.10.8
  config file = None
  configured module search path = ['/home/vagrant/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  executable location = /usr/bin/ansible
  python version = 3.10.12 (main, Aug 15 2025, 14:32:43) [GCC 11.4.0]
```

capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP2_installer_ansible/tp2_1_6_Ian-Bertin_Hugo-Crepin.md

**8 - Déconnectez-vous et supprimez la VM :**

Réponses :
* ```exit```
* ```vagrant destroy -f ubuntu```

capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP2_installer_ansible/tp2_1_7_Ian-Bertin_Hugo-Crepin.md