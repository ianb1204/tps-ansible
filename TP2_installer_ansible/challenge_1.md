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

**2 - Connectez-vous à cette VM :**

Réponse : ```vagrant ssh ubuntu```

**3 - Rafraîchissez les informations sur les paquets :**

Réponse : ```sudo apt update```

**4 - Recherchez le paquet ansible avec les options qui vont bien :**

Réponse : ```apt-cache search --names-only ansible```

**5 - Installez le paquet officiel fourni par la distribution :**

Réponse : ```sudo apt install -y ansible```

**6 - Vérifiez si l'installation s'est bien déroulée :**

Réponse : ```ansible --version```

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

**8 - Déconnectez-vous et supprimez la VM :**

Réponses :
* ```exit```
* ```vagrant destroy -f ubuntu```