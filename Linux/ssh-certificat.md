---
description: Se connecter en ssh à un server distant à l'aide d'une clé publique (sans login/mot de passe).
categories: [GNU/Linux]
date: 2021-10-21
tags: [GNU/Linux, debian, ssh, password, key]
order: 43
---

# :ballot_box_with_ballot: Connexion SSH avec clé publique

:icon-list-unordered: Se connecter avec une clé publqiue plutôt qu'un user/password représente plusieurs avantages:

:icon-triangle-right: Une clé générée sera toujours plus robuste qu'un mot de passe de manière générale  
:icon-triangle-right: Il n'y a pas besoin d'utiliser de mot de passe pour se connecter en ssh  

---

## :key: Génération de la clé

:icon-triangle-right: Chacune de ses commandes peut être utilisée pour définir le type de chiffrement utilisé pour communiquer avec SSH:  

```
ssh-keygen -t rsa -b 4096
ssh-keygen -t dsa 
ssh-keygen -t ecdsa -b 521
ssh-keygen -t ed25519
```

:icon-info: Il peut être nécessaire d'utiliser tel ou tel type de clé selon le type de chiffrement accepté par le serveur auquel vous vous connectez.

!!!success
Il est recommandé d'utiliser `ed25519` qui correspond au standard actuel en terme de sécurité.
!!!

``` ssh-keygen ed25519
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/contact/.ssh/id_ed25519):
Created directory '/home/contact/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/contact/.ssh/id_ed25519
Your public key has been saved in /home/contact/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:monhash contact@mondomain
The key's randomart image is:
+--[ED25519 256]--+
|%@+.             |
|EBCVB.             |
|+=Bo.            |
|+.o* .           |
|oo..o . S S       |
|=+oo   o         |
|B++ o . .        |
|+=.. o .         |
|o.    . .         |
+----[SHA256]-----+
```

---

## :page_facing_up: Renseigner les informations de connexion

```
ssh-copy-id contact@mondomain
```

`ssh-copy-id nom_user@IP_OU_DOMAINE`

``` ssh-copy-id admin@dns.it.fr
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/root/.ssh/id_rsa.pub"
The authenticity of host 'dns.it.fr (172.16.10.10)' can't be established.
ECDSA key fingerprint is SHA256:LdA9PSN+R3COe3P2eH2HDar6F50GatnLuTf5hw+qQkA.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
admin@dns.it.fr's password:

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'admin@dns.it.fr'"
and check to make sure that only the key(s) you wanted were added.
```

---

## :closed_lock_with_key: Se connecter en ssh

```
ssh admin@dns.it.fr
```

`ssh nom_user@IP_OU_DOMAINE`

!!! success
Vous êtes maintenant connecté en SHH grâce à la clé d'autorisation.
!!!

==- :icon-sort-asc: Consulter/ exporter la clé d'autorisation

```
cat .ssh/authorized_keys
```
``` cat .ssh/authorized_keys
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC/3HQOjnQS9OCUAKIfaMoB4twLZfsGf50Vao7h7aT3
fiMZ+HQekttMds8CXojaHo1j+4z4+2HFhbvO5exxl4/HMyZtMbJzveImAk+J5uxnrRzX6eBVL6Btzg7W
Ek5IfRRTbLviDGkQri4e1Dv8YC7NUPDQbaGTIs3+UW3miWkKny6hvgJ6TiqfOxT1eLRI38CvUX26PcUG
6FbvhQxoXtBWLojuXzYD2nAYH4zlVYr34ktPYcNkAoVfAYrSNbVcq3A7xQWLuurPYOajtnDwCbUi4eKO
iStFqvgC7BT9sCPmYYklb+D58QoWl74vELoqirUDGc0O66ZX3BCtKU6Dw9aGi/LwDoDJLqhQQlIkDIHk
G15vIgNcNtCY4F9P+Kcg3g2Tp+KNXmmBTuWll8mMa6odLGLftPaXjXJS+L9cpeOUWt2SkGHHgXpuUXfL
LmtnqFSjmTbzhH5YWb0eQ8FyVnBiSJ02T0JD8GoQ6qcIa+7RWOu15vu+Hip5QrCu17CEJZE= root@
pc-admin
```

==- 
