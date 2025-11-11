TP5 : Idempotence
---

### Challenge :

**Consignes :** 

* Installez successivement les paquets ```tree```, ```git``` et ```nmap``` sur toutes les cibles.
* Désinstallez successivement ces trois paquets en utilisant le paramètre supplémentaire ```state=absent```.
* Copiez le fichier ```/etc/fstab``` du _Control Host_ vers tous les _Target Hosts_ sous forme d'un fichier ```/tmp/test3.txt```.
* Supprimez le fichier ```/tmp/test3.txt``` sur les _Target Hosts_ en utilisant le module ```file``` avec le paramètre ```state=absent```.
* Enfin, affichez l'espace utilisé par la partition principale sur tous les _Target Hosts_. Que remarquez-vous ?

---

#### Réponses : 

**1 - Installez successivement les paquets ```tree```, ```git``` et ```nmap``` sur toutes les cibles :**

On commence par initialiser l'espace de travail :

```
$ cd atelier-07/
$ vagrant up
```

Puis on s'y connecte et on se place dans le repertoire de travail dans lequel se trouve le ansible.cfg :

```
$ vagrant ssh ansible
$ cd ansible/projets/ema
```

capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP5_idempotence/screenshots/tp5_1_Ian-Bertin_Hugo-Crepin.png

Enfin, j'y install ```tree``` et ```nmap```, l'installation de ```git``` étant impossible à cause du problème vu en cours :

```
$ ansible all -m package -a "name=tree"
$ ansible all -m package -a "name=nmap"
```

Exemple de réponse (```tree```) :
```
debian | CHANGED => {
    "cache_update_time": 1761245363,
    "cache_updated": false,
    "changed": true,
    "stderr": "",
    "stderr_lines": [],
    "stdout": "Reading package lists...\nBuilding dependency tree...\nReading state information...\nThe following NEW packages will be installed:\n  tree\n0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.\nNeed to get 52.5 kB of archives.\nAfter this operation, 116 kB of additional disk space will be used.\nGet:1 http://httpredir.debian.org/debian bookworm/main amd64 tree amd64 2.1.0-1 [52.5 kB]\nFetched 52.5 kB in 0s (137 kB/s)\nSelecting previously unselected package tree.\r\n(Reading database ... \r(Reading database ... 5%\r(Reading database ... 10%\r(Reading database ... 15%\r(Reading database ... 20%\r(Reading database ... 25%\r(Reading database ... 30%\r(Reading database ... 35%\r(Reading database ... 40%\r(Reading database ... 45%\r(Reading database ... 50%\r(Reading database ... 55%\r(Reading database ... 60%\r(Reading database ... 65%\r(Reading database ... 70%\r(Reading database ... 75%\r(Reading database ... 80%\r(Reading database ... 85%\r(Reading database ... 90%\r(Reading database ... 95%\r(Reading database ... 100%\r(Reading database ... 34138 files and directories currently installed.)\r\nPreparing to unpack .../tree_2.1.0-1_amd64.deb ...\r\nUnpacking tree (2.1.0-1) ...\r\nSetting up tree (2.1.0-1) ...\r\nProcessing triggers for man-db (2.11.2-2) ...\r\n",
    "stdout_lines": [
        "Reading package lists...",
        "Building dependency tree...",
        "Reading state information...",
        "The following NEW packages will be installed:",
        "  tree",
        "0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.",
        "Need to get 52.5 kB of archives.",
        "After this operation, 116 kB of additional disk space will be used.",
        "Get:1 http://httpredir.debian.org/debian bookworm/main amd64 tree amd64 2.1.0-1 [52.5 kB]",
        "Fetched 52.5 kB in 0s (137 kB/s)",
        "Selecting previously unselected package tree.",
        "(Reading database ... ",
        "(Reading database ... 5%",
        "(Reading database ... 10%",
        "(Reading database ... 15%",
        "(Reading database ... 20%",
        "(Reading database ... 25%",
        "(Reading database ... 30%",
        "(Reading database ... 35%",
        "(Reading database ... 40%",
        "(Reading database ... 45%",
        "(Reading database ... 50%",
        "(Reading database ... 55%",
        "(Reading database ... 60%",
        "(Reading database ... 65%",
        "(Reading database ... 70%",
        "(Reading database ... 75%",
        "(Reading database ... 80%",
        "(Reading database ... 85%",
        "(Reading database ... 90%",
        "(Reading database ... 95%",
        "(Reading database ... 100%",
        "(Reading database ... 34138 files and directories currently installed.)",
        "Preparing to unpack .../tree_2.1.0-1_amd64.deb ...",
        "Unpacking tree (2.1.0-1) ...",
        "Setting up tree (2.1.0-1) ...",
        "Processing triggers for man-db (2.11.2-2) ..."
    ]
}
rocky | SUCCESS => {
    "changed": false,
    "msg": "Nothing to do",
    "rc": 0,
    "results": []
}
suse | CHANGED => {
    "changed": true,
    "cmd": [
        "/usr/bin/zypper",
        "--quiet",
        "--non-interactive",
        "--xmlout",
        "install",
        "--type",
        "package",
        "--auto-agree-with-licenses",
        "--no-recommends",
        "--",
        "+tree"
    ],
    "name": [
        "tree"
    ],
    "rc": 0,
    "state": "present",
    "stderr": "",
    "stderr_lines": [],
    "stdout": "<?xml version='1.0'?>\n<stream>\n<install-summary download-size=\"56772\" space-usage-diff=\"116508\" space-usage-installed=\"116508\" space-usage-removed=\"0\" packages-to-change=\"1\" need-restart=\"0\" need-reboot=\"0\">\n<to-install>\n<solvable type=\"package\" name=\"tree\" edition=\"1.7.0-150000.3.2.1\" arch=\"x86_64\" repository=\"repo-sle-update\" summary=\"File listing as a tree\">\n<description>Tree is a recursive directory listing command that produces a depth\nindented listing of files, which is colorized ala dircolors if the\nLS_COLORS environment variable is set and output is to tty.</description></solvable>\n</to-install>\n</install-summary>\n<prompt id=\"0\">\n<text>Backend:  classic_rpmtrans\nContinue?</text>\n<option default=\"1\" value=\"y\" desc=\"Yes, accept the summary and proceed with installation/removal of packages.\"/>\n<option value=\"n\" desc=\"No, cancel the operation.\"/>\n<option value=\"v\" desc=\"Toggle display of package versions.\"/>\n<option value=\"a\" desc=\"Toggle display of package architectures.\"/>\n<option value=\"r\" desc=\"Toggle display of repositories from which the packages will be installed.\"/>\n<option value=\"m\" desc=\"Toggle display of package vendor names.\"/>\n<option value=\"d\" desc=\"Toggle between showing all details and as few details as possible.\"/>\n<option value=\"g\" desc=\"View the summary in pager.\"/>\n</prompt>\n<download url=\"http://fr2.rpmfind.net/linux/opensuse/update/leap/15.6/sle/x86_64/tree-1.7.0-150000.3.2.1.x86_64.rpm\" percent=\"-1\" rate=\"-1\"/>\n<download url=\"http://fr2.rpmfind.net/linux/opensuse/update/leap/15.6/sle/x86_64/tree-1.7.0-150000.3.2.1.x86_64.rpm\" percent=\"0\" rate=\"0\"/>\n<download url=\"http://fr2.rpmfind.net/linux/opensuse/update/leap/15.6/sle/x86_64/tree-1.7.0-150000.3.2.1.x86_64.rpm\" percent=\"0\" rate=\"0\"/>\n<download url=\"http://fr2.rpmfind.net/linux/opensuse/update/leap/15.6/sle/x86_64/tree-1.7.0-150000.3.2.1.x86_64.rpm\" percent=\"4\" rate=\"2516\"/>\n<download url=\"http://fr2.rpmfind.net/linux/opensuse/update/leap/15.6/sle/x86_64/tree-1.7.0-150000.3.2.1.x86_64.rpm\" percent=\"41\" rate=\"2516\"/>\n<download url=\"http://fr2.rpmfind.net/linux/opensuse/update/leap/15.6/sle/x86_64/tree-1.7.0-150000.3.2.1.x86_64.rpm\" percent=\"100\" rate=\"2516\"/>\n<download url=\"http://fr2.rpmfind.net/linux/opensuse/update/leap/15.6/sle/x86_64/tree-1.7.0-150000.3.2.1.x86_64.rpm\" rate=\"56772\" done=\"1\"/>\n</stream>\n",
    "stdout_lines": [
        "<?xml version='1.0'?>",
        "<stream>",
        "<install-summary download-size=\"56772\" space-usage-diff=\"116508\" space-usage-installed=\"116508\" space-usage-removed=\"0\" packages-to-change=\"1\" need-restart=\"0\" need-reboot=\"0\">",
        "<to-install>",
        "<solvable type=\"package\" name=\"tree\" edition=\"1.7.0-150000.3.2.1\" arch=\"x86_64\" repository=\"repo-sle-update\" summary=\"File listing as a tree\">",
        "<description>Tree is a recursive directory listing command that produces a depth",
        "indented listing of files, which is colorized ala dircolors if the",
        "LS_COLORS environment variable is set and output is to tty.</description></solvable>",
        "</to-install>",
        "</install-summary>",
        "<prompt id=\"0\">",
        "<text>Backend:  classic_rpmtrans",
        "Continue?</text>",
        "<option default=\"1\" value=\"y\" desc=\"Yes, accept the summary and proceed with installation/removal of packages.\"/>",
        "<option value=\"n\" desc=\"No, cancel the operation.\"/>",
        "<option value=\"v\" desc=\"Toggle display of package versions.\"/>",
        "<option value=\"a\" desc=\"Toggle display of package architectures.\"/>",
        "<option value=\"r\" desc=\"Toggle display of repositories from which the packages will be installed.\"/>",
        "<option value=\"m\" desc=\"Toggle display of package vendor names.\"/>",
        "<option value=\"d\" desc=\"Toggle between showing all details and as few details as possible.\"/>",
        "<option value=\"g\" desc=\"View the summary in pager.\"/>",
        "</prompt>",
        "<download url=\"http://fr2.rpmfind.net/linux/opensuse/update/leap/15.6/sle/x86_64/tree-1.7.0-150000.3.2.1.x86_64.rpm\" percent=\"-1\" rate=\"-1\"/>",
        "<download url=\"http://fr2.rpmfind.net/linux/opensuse/update/leap/15.6/sle/x86_64/tree-1.7.0-150000.3.2.1.x86_64.rpm\" percent=\"0\" rate=\"0\"/>",
        "<download url=\"http://fr2.rpmfind.net/linux/opensuse/update/leap/15.6/sle/x86_64/tree-1.7.0-150000.3.2.1.x86_64.rpm\" percent=\"0\" rate=\"0\"/>",
        "<download url=\"http://fr2.rpmfind.net/linux/opensuse/update/leap/15.6/sle/x86_64/tree-1.7.0-150000.3.2.1.x86_64.rpm\" percent=\"4\" rate=\"2516\"/>",
        "<download url=\"http://fr2.rpmfind.net/linux/opensuse/update/leap/15.6/sle/x86_64/tree-1.7.0-150000.3.2.1.x86_64.rpm\" percent=\"41\" rate=\"2516\"/>",
        "<download url=\"http://fr2.rpmfind.net/linux/opensuse/update/leap/15.6/sle/x86_64/tree-1.7.0-150000.3.2.1.x86_64.rpm\" percent=\"100\" rate=\"2516\"/>",
        "<download url=\"http://fr2.rpmfind.net/linux/opensuse/update/leap/15.6/sle/x86_64/tree-1.7.0-150000.3.2.1.x86_64.rpm\" rate=\"56772\" done=\"1\"/>",
        "</stream>"
    ],
    "update_cache": false
}
```

On vérifie l'instalation :
```
$ ansible all -m shell -a 'tree ~ executable=/bin/bash'

rocky | CHANGED | rc=0 >>
/root

0 directories, 0 files
debian | CHANGED | rc=0 >>
/root

0 directories, 0 files
suse | CHANGED | rc=0 >>
/root
├── bin
└── inst-sys

2 directories, 0 files

$ ansible all -m shell -a 'nmap ~ executable=/bin/bash'

debian | CHANGED | rc=0 >>
Starting Nmap 7.93 ( https://nmap.org ) at 2025-11-11 06:57 UTC
Nmap done: 0 IP addresses (0 hosts up) scanned in 0.03 secondsUnable to split netmask from target expression: "/root"
WARNING: No targets were specified, so 0 hosts scanned.
suse | CHANGED | rc=0 >>
Starting Nmap 7.94 ( https://nmap.org ) at 2025-11-11 06:57 UTC
Nmap done: 0 IP addresses (0 hosts up) scanned in 0.02 secondsUnable to split netmask from target expression: "/root"
WARNING: No targets were specified, so 0 hosts scanned.
rocky | CHANGED | rc=0 >>
Starting Nmap 7.92 ( https://nmap.org ) at 2025-11-11 06:57 UTC
Nmap done: 0 IP addresses (0 hosts up) scanned in 0.03 secondsUnable to split netmask from target expression: "/root"
WARNING: No targets were specified, so 0 hosts scanned.
```

capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP5_idempotence/screenshots/tp5_2_Ian-Bertin_Hugo-Crepin.png

**2 - Désinstallez successivement ces trois paquets en utilisant le paramètre supplémentaire ```state=absent``` :**

```
ansible all -m package -a "name=tree state=absent"
ansible all -m package -a "name=nmap state=absent"
```

Exemple de réponse (```tree```) :
```
suse | CHANGED => {
    "changed": true,
    "cmd": [
        "/usr/bin/zypper",
        "--quiet",
        "--non-interactive",
        "--xmlout",
        "remove",
        "--type",
        "package",
        "tree"
    ],
    "name": [
        "tree"
    ],
    "rc": 0,
    "state": "absent",
    "stderr": "",
    "stderr_lines": [],
    "stdout": "<?xml version='1.0'?>\n<stream>\n<install-summary download-size=\"0\" space-usage-diff=\"-116508\" space-usage-installed=\"0\" space-usage-removed=\"116508\" packages-to-change=\"1\" need-restart=\"0\" need-reboot=\"0\">\n<to-remove>\n<solvable type=\"package\" name=\"tree\" edition=\"1.7.0-150000.3.2.1\" arch=\"x86_64\" repository=\"@System\" summary=\"File listing as a tree\">\n<description>Tree is a recursive directory listing command that produces a depth\nindented listing of files, which is colorized ala dircolors if the\nLS_COLORS environment variable is set and output is to tty.</description></solvable>\n</to-remove>\n</install-summary>\n<prompt id=\"0\">\n<text>Backend:  classic_rpmtrans\nContinue?</text>\n<option default=\"1\" value=\"y\" desc=\"Yes, accept the summary and proceed with installation/removal of packages.\"/>\n<option value=\"n\" desc=\"No, cancel the operation.\"/>\n<option value=\"v\" desc=\"Toggle display of package versions.\"/>\n<option value=\"a\" desc=\"Toggle display of package architectures.\"/>\n<option value=\"r\" desc=\"Toggle display of repositories from which the packages will be installed.\"/>\n<option value=\"m\" desc=\"Toggle display of package vendor names.\"/>\n<option value=\"d\" desc=\"Toggle between showing all details and as few details as possible.\"/>\n<option value=\"g\" desc=\"View the summary in pager.\"/>\n</prompt>\n</stream>\n",
    "stdout_lines": [
        "<?xml version='1.0'?>",
        "<stream>",
        "<install-summary download-size=\"0\" space-usage-diff=\"-116508\" space-usage-installed=\"0\" space-usage-removed=\"116508\" packages-to-change=\"1\" need-restart=\"0\" need-reboot=\"0\">",
        "<to-remove>",
        "<solvable type=\"package\" name=\"tree\" edition=\"1.7.0-150000.3.2.1\" arch=\"x86_64\" repository=\"@System\" summary=\"File listing as a tree\">",
        "<description>Tree is a recursive directory listing command that produces a depth",
        "indented listing of files, which is colorized ala dircolors if the",
        "LS_COLORS environment variable is set and output is to tty.</description></solvable>",
        "</to-remove>",
        "</install-summary>",
        "<prompt id=\"0\">",
        "<text>Backend:  classic_rpmtrans",
        "Continue?</text>",
        "<option default=\"1\" value=\"y\" desc=\"Yes, accept the summary and proceed with installation/removal of packages.\"/>",
        "<option value=\"n\" desc=\"No, cancel the operation.\"/>",
        "<option value=\"v\" desc=\"Toggle display of package versions.\"/>",
        "<option value=\"a\" desc=\"Toggle display of package architectures.\"/>",
        "<option value=\"r\" desc=\"Toggle display of repositories from which the packages will be installed.\"/>",
        "<option value=\"m\" desc=\"Toggle display of package vendor names.\"/>",
        "<option value=\"d\" desc=\"Toggle between showing all details and as few details as possible.\"/>",
        "<option value=\"g\" desc=\"View the summary in pager.\"/>",
        "</prompt>",
        "</stream>"
    ],
    "update_cache": false
}
debian | CHANGED => {
    "changed": true,
    "stderr": "",
    "stderr_lines": [],
    "stdout": "Reading package lists...\nBuilding dependency tree...\nReading state information...\nThe following packages will be REMOVED:\n  tree\n0 upgraded, 0 newly installed, 1 to remove and 0 not upgraded.\nAfter this operation, 116 kB disk space will be freed.\n(Reading database ... \r(Reading database ... 5%\r(Reading database ... 10%\r(Reading database ... 15%\r(Reading database ... 20%\r(Reading database ... 25%\r(Reading database ... 30%\r(Reading database ... 35%\r(Reading database ... 40%\r(Reading database ... 45%\r(Reading database ... 50%\r(Reading database ... 55%\r(Reading database ... 60%\r(Reading database ... 65%\r(Reading database ... 70%\r(Reading database ... 75%\r(Reading database ... 80%\r(Reading database ... 85%\r(Reading database ... 90%\r(Reading database ... 95%\r(Reading database ... 100%\r(Reading database ... 35072 files and directories currently installed.)\r\nRemoving tree (2.1.0-1) ...\r\nProcessing triggers for man-db (2.11.2-2) ...\r\n",
    "stdout_lines": [
        "Reading package lists...",
        "Building dependency tree...",
        "Reading state information...",
        "The following packages will be REMOVED:",
        "  tree",
        "0 upgraded, 0 newly installed, 1 to remove and 0 not upgraded.",
        "After this operation, 116 kB disk space will be freed.",
        "(Reading database ... ",
        "(Reading database ... 5%",
        "(Reading database ... 10%",
        "(Reading database ... 15%",
        "(Reading database ... 20%",
        "(Reading database ... 25%",
        "(Reading database ... 30%",
        "(Reading database ... 35%",
        "(Reading database ... 40%",
        "(Reading database ... 45%",
        "(Reading database ... 50%",
        "(Reading database ... 55%",
        "(Reading database ... 60%",
        "(Reading database ... 65%",
        "(Reading database ... 70%",
        "(Reading database ... 75%",
        "(Reading database ... 80%",
        "(Reading database ... 85%",
        "(Reading database ... 90%",
        "(Reading database ... 95%",
        "(Reading database ... 100%",
        "(Reading database ... 35072 files and directories currently installed.)",
        "Removing tree (2.1.0-1) ...",
        "Processing triggers for man-db (2.11.2-2) ..."
    ]
}
rocky | CHANGED => {
    "changed": true,
    "msg": "",
    "rc": 0,
    "results": [
        "Removed: tree-1.8.0-10.el9.x86_64"
    ]
}
```

On vérifie la desinstallation :

```
$ ansible all -m shell -a 'tree ~ executable=/bin/bash'

debian | FAILED | rc=127 >>
/bin/bash: line 1: tree: command not foundnon-zero return code
suse | FAILED | rc=127 >>
/bin/bash: tree: command not foundnon-zero return code
rocky | FAILED | rc=127 >>
/bin/bash: line 1: tree: command not foundnon-zero return code

$ ansible all -m shell -a 'nmap ~ executable=/bin/bash'

debian | FAILED | rc=127 >>
/bin/bash: line 1: nmap: command not foundnon-zero return code
suse | FAILED | rc=127 >>
/bin/bash: nmap: command not foundnon-zero return code
rocky | FAILED | rc=127 >>
/bin/bash: line 1: nmap: command not foundnon-zero return code
```

capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP5_idempotence/screenshots/tp5_3_Ian-Bertin_Hugo-Crepin.png

**3 - Copiez le fichier ```/etc/fstab``` du _Control Host_ vers tous les _Target Hosts_ sous forme d'un fichier ```/tmp/test3.txt``` :**

```
$ ansible all -m copy -a "src=/etc/fstab dest=/tmp/test3.txt"

debian | CHANGED => {
    "changed": true,
    "checksum": "0fe1d6fcaf1695fb3ef9d8a42d45d04e5e0c11c2",
    "dest": "/tmp/test3.txt",
    "gid": 0,
    "group": "root",
    "md5sum": "31623c38118cbe8247061a6bd3f239a4",
    "mode": "0644",
    "owner": "root",
    "size": 679,
    "src": "/home/vagrant/.ansible/tmp/ansible-tmp-1762844591.2936225-6526-17300453319/source",
    "state": "file",
    "uid": 0
}
rocky | CHANGED => {
    "changed": true,
    "checksum": "0fe1d6fcaf1695fb3ef9d8a42d45d04e5e0c11c2",
    "dest": "/tmp/test3.txt",
    "gid": 0,
    "group": "root",
    "md5sum": "31623c38118cbe8247061a6bd3f239a4",
    "mode": "0644",
    "owner": "root",
    "secontext": "unconfined_u:object_r:user_home_t:s0",
    "size": 679,
    "src": "/home/vagrant/.ansible/tmp/ansible-tmp-1762844591.2813132-6525-148547660358566/source",
    "state": "file",
    "uid": 0
}
suse | CHANGED => {
    "changed": true,
    "checksum": "0fe1d6fcaf1695fb3ef9d8a42d45d04e5e0c11c2",
    "dest": "/tmp/test3.txt",
    "gid": 0,
    "group": "root",
    "md5sum": "31623c38118cbe8247061a6bd3f239a4",
    "mode": "0644",
    "owner": "root",
    "size": 679,
    "src": "/home/vagrant/.ansible/tmp/ansible-tmp-1762844591.2904735-6527-220077472541373/source",
    "state": "file",
    "uid": 0
}
```

capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP5_idempotence/screenshots/tp5_4_Ian-Bertin_Hugo-Crepin.png

**4 - Supprimez le fichier ```/tmp/test3.txt``` sur les _Target Hosts_ en utilisant le module ```file``` avec le paramètre ```state=absent``` :**

```
$ ansible all -m file -a "dest=/tmp/test3.txt state=absent"

debian | CHANGED => {
    "changed": true,
    "path": "/tmp/test3.txt",
    "state": "absent"
}
rocky | CHANGED => {
    "changed": true,
    "path": "/tmp/test3.txt",
    "state": "absent"
}
suse | CHANGED => {
    "changed": true,
    "path": "/tmp/test3.txt",
    "state": "absent"
}
```

capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP5_idempotence/screenshots/tp5_5_Ian-Bertin_Hugo-Crepin.png

**5 - Enfin, affichez l'espace utilisé par la partition principale sur tous les _Target Hosts_. Que remarquez-vous ?**

```
$ ansible all -m command -a "df -h /"

suse | CHANGED | rc=0 >>
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda3        64G  2.3G   58G   4% /
rocky | CHANGED | rc=0 >>
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda2        61G  2.0G   59G   4% /
debian | CHANGED | rc=0 >>
Filesystem                       Size  Used Avail Use% Mounted on
/dev/mapper/debian--12--vg-root   62G  1.7G   57G   3% /
```

capture d'écran : https://github.com/ianb1204/tps-ansible/blob/main/TP5_idempotence/screenshots/tp5_6_Ian-Bertin_Hugo-Crepin.png

Conclusion : le status est toujours ```CHANGED```, donc la commande de type ```command``` n'est pas idempotente par nature.