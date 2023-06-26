---
description: Créer un serveur openVPN sur debian et s'y connecter avec un client.
categories: [GNU/Linux]
date: 2021-10-21
tags: [GNU/Linux, debian, openvpn, vpn, server, client]
order: 45
---

# :closed_lock_with_key: Installation d'un serveur OpenVPN sur debian

==- :icon-report: Mises en garde

!!! primary
Vous pouvez directement utiliser la barre de recherche.  
Ou l'onglet de droite pour trouver la section qui vous intéresse.
!!!

!!! danger
Toutes les adresses ip et masques de cette documentation, sont proposés à titre d'exemple, et peuvent donc, ne pas coïncider avec votre configuration.  
Faites attention, à bien renseigner les vôtres :slightly_smiling_face:
!!!

==-

---

## :file_cabinet: Configuration du serveur

---

### :inbox_tray: Installer OpenVPN sur le serveur

:icon-diff-removed: Se connecter en root

```
su -
```

:icon-download: Installer OpenVPN

```
apt update && apt install openvpn
```

---

### :memo: Copier le répertoire easy-rsa

:icon-file-badge: easy-rsa permet de générer les certificats.

```
cp -pr /usr/share/easy-rsa /etc/openvpn/server/ && cd /etc/openvpn/server/easy-rsa
```

```
/  
└── 📁etc
    └── 📁openvpn
        └── 📁server
            └── 📁easy-rsa
```

---

### :memo: Renommer et éditer le fichier vars à partir du template

```
cp vars.example vars && nano vars
```

```
/  
└── 📁etc
    └── 📁openvpn
        └── 📁server
            └── 📁easy-rsa
                ├── 📄vars.example
                └── 📄vars
```

:mag_right: Rechercher le bloc suivant:

``` /etc/openvpn/server/easy-rsa/vars
# Organizational fields (used with 'org' mode and ignored in 'cn_only' mode.)
# These are the default values for fields which will be placed in the
# certificate.  Don't leave any of these fields blank, although interactively
# you may omit any specific field by typing the "." symbol (not valid for
# email.)

#set_var EASYRSA_REQ_COUNTRY    "US"
#set_var EASYRSA_REQ_PROVINCE   "California"
#set_var EASYRSA_REQ_CITY       "San Francisco"
#set_var EASYRSA_REQ_ORG        "Copyleft Certificate Co"
#set_var EASYRSA_REQ_EMAIL      "me@example.net"
#set_var EASYRSA_REQ_OU         "My Organizational Unit"

# Choose a size in bits for your keypairs. The recommended value is 2048.  Using
# 2048-bit keys is considered more than sufficient for many years into the
# future. Larger keysizes will slow down TLS negotiation and make key/DH param
# generation take much longer. Values up to 4096 should be accepted by most
# software. Only used when the crypto alg is rsa (see below.)
```

- Décommenter les lignes et renseigner votre configuration:

``` /etc/openvpn/server/easy-rsa/vars
set_var EASYRSA_REQ_COUNTRY     "FR"
set_var EASYRSA_REQ_PROVINCE    "France"
set_var EASYRSA_REQ_CITY        "maville"
set_var EASYRSA_REQ_ORG "Contactit"
set_var EASYRSA_REQ_EMAIL       "contactit.yarka@slmail.me"
set_var EASYRSA_REQ_OU          "it"
```

---

### :sleuth_or_spy: Créer l'autorité de certification

:icon-shield: Ici sans mot de passe, en production il est recommandé d'en mettre un.

```
./easyrsa init-pki
```

```
./easyrsa build-ca nopass 
```

``` ./easyrsa build-ca nopass 
Note: using Easy-RSA configuration from: /etc/openvpn/server/easy-rsa/vars
Using SSL: openssl OpenSSL 1.1.1k  25 Mar 2021
Generating RSA private key, 2048 bit long modulus (2 primes)
.............................+++++
..................................+++++
e is 65537 (0x010001)
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Common Name (eg: your user, host, or server name) [Easy-RSA CA]:

CA creation complete and you may now import and sign cert requests.
Your new CA certificate file for publishing is at:
/etc/openvpn/server/easy-rsa/pki/ca.crt
```

- `Common Name [Easy-RSA CA]:` Entrer pour laisser le nom par defaut.

---

### :page_with_curl: Générer le certificat du serveur

:icon-shield: Ici sans mot de passe, en production il est recommandé d'en mettre un.

```
./easyrsa build-server-full server nopass
```

---

### :page_with_curl: Générer le certificat du client

:icon-shield: Ici sans mot de passe, en production il est recommandé d'en mettre un.

```
./easyrsa build-client-full client nopass
```

---

### :page_with_curl: Générer le fichier dh.pem

:icon-key: Ce fichier va être utilisé pour la première connexion en chiffrement symétrique

```
./easyrsa gen-dh
```

:icon-issue-reopened: Cette opération peut prendre du temps, cela dépend de la puissance de votre machine.

---

### :key: Générer le fichier clé

```
openvpn --genkey tls-auth ta.key
```

---

### :card_index_dividers: Remise en ordre des fichiers

:icon-duplicate: Copier l'ensemble des fichiers générés du serveur et de l'autorité de certification dans `/etc/openvpn/`

```
cp pki/issued/server.crt pki/private/server.key pki/ca.crt pki/dh.pem ta.key /etc/openvpn/
```

```
/
└── 📁etc
    └── 📁openvpn
        ├── 📄server.crt
        ├── 📄server.key
        ├── 📄ca.crt
        ├── 📄dh.pem
        └── 📄ta.key
```

:icon-duplicate: Copier l'ensemble des fichiers générés du client dans `/etc/openvpn/client/`

```
cp pki/issued/client.crt pki/private/client.key pki/ca.crt pki/dh.pem ta.key /etc/openvpn/client/
```

```
/
└── 📁etc
    └── 📁openvpn
        └── 📁client
            ├── 📄client.crt
            ├── 📄client.key
            ├── 📄ca.crt
            ├── 📄dh.pem
            └── 📄ta.key
```

---

### :bookmark_tabs: Créer le fichier de configuration à partir du template

```
cp /usr/share/doc/openvpn/examples/sample-config-files/server.conf /etc/openvpn/server.conf && cd /etc/openvpn
```

```
/
└── 📁etc
    └── 📁openvpn
        ├── 📄server.conf
		├── 📄server.crt
        ├── 📄server.key
        ├── 📄ca.crt
        ├── 📄dh.pem
        └── 📄ta.key
```

:icon-sign-out: Ici on copie le fichier de configuration dans le répertoire par défaut vers `/etc/openvpn/` et on rentre dans le répertoire.

---

### :pencil2: Renommer le fichier dh.pem 

```
mv /etc/openvpn/dh.pem /etc/openvpn/dh2048.pem
```

!!!
En le renommant `dh2048.pem` le fichier `server.conf` le reconnaîtra directement, puisque le nom de fichier `dh2048.pem` y est renseigné par défaut.
!!!

---

### :heavy_check_mark: Tester la configuration

```
openvpn --config /etc/openvpn/server.conf
```

!!! success
Si votre configuration est correcte, vous verrez en dernière ligne:

```
Initialization Sequence Completed
```
!!!

==- :x: Erreur "Already in use"

!!! danger
:information_source: Si vous avez deja une instance d'openvpn qui tourne, il peux y avoir un conflit avec l'installation (erreur: Alredy in use).  
Il vous faut trouver le processus qui bloque le port du service.  

Vous pouvez lister la liste des ports utilisés:  

```
ss -na
```
!!!

:information_source: Vous pouvez filtrer le port par défaut d'OpenVPN, pour voir si une instance tourne:

```
ss -pan | grep 1194
udp   UNCONN 0      0                                         0.0.0.0:1194             0.0.0.0:*      users:(("openvpn",pid=8660,fd=7))
```

- Vous pouvez voir le `pid=8660` iCi c'est ce numéro de processus que vous devez arrêter:

```
kill -9 8660
```

- Recommencer l'opération jusqu'à ce que `ss -pan | grep 1194` ne retourne plus de résultat (en adaptant le pid à chaque fois)

==-

---

### :arrows_counterclockwise: Redémarrer openvpn sur le serveur

```
systemctl daemon-reload && systemctl restart openvpn
```

!!!
Vous pouvez ajouter `systemctl enable openvpn` pour qu'OpenVPN démarre automatiquement avec la machine.
!!!

---

## :woman-raising-hand: Configuration du client

---

### :inbox_tray: Installer openvpn sur le client

:icon-diff-removed: Se connecter en root

```
su -
```

```
apt update && apt install openvpn
```

---

### :bookmark_tabs: Copier les fichiers du répertoire /etc/openvpn/client pour les transférer sur la machine client

:icon-chevron-right: Sur la machine client, il faut ensuite les déplacer dans `/etc/openvpn/`

```
/
└── 📁etc
    └── 📁openvpn
        ├── 📄client.crt
        ├── 📄client.key
        ├── 📄ca.crt
        ├── 📄dh.pem
        └── 📄ta.key
```

---

### :bookmark_tabs: Copier et éditer le fichier de conf client

```
cp /usr/share/doc/openvpn/examples/sample-config-files/client.conf /etc/openvpn/ && nano /etc/openvpn/client.conf
```

```
/
└── 📁etc
    └── 📁openvpn
        └── 📄client.conf
```

``` /etc/openvpn/client.conf
##############################################
# Sample client-side OpenVPN 2.0 config file #
# for connecting to multi-client server.     #
#                                            #
# This configuration can be used by multiple #
# clients, however each client should have   #
# its own cert and key files.                #
#                                            #
# On Windows, you might want to rename this  #
# file so it has a .ovpn extension           #
##############################################

# Specify that we are a client and that we
# will be pulling certain config file directives
# from the server.
client

# Use the same setting as you are using on
# the server.
# On most systems, the VPN will not function
# unless you partially or fully disable
# the firewall for the TUN/TAP interface.
;dev tap
dev tun

# Windows needs the TAP-Win32 adapter name
# from the Network Connections panel
# if you have more than one.  On XP SP2,
# you may need to disable the firewall
# for the TAP adapter.
;dev-node MyTap

# Are we connecting to a TCP or
# UDP server?  Use the same setting as
# on the server.
;proto tcp
proto udp

# The hostname/IP and port of the server.
# You can have multiple remote entries
# to load balance between the servers.
remote my-server-1 1194
;remote my-server-2 1194
```

:icon-quote: Remplacer `remote my-server-1 1194` par `remote IP_SERVEUR_OPENVPN 1194`

---

### :arrows_counterclockwise: Redémarrer OpenVPN sur le client

```
systemctl daemon-reload && systemctl restart openvpn
```

!!!
Vous pouvez ajouter `systemctl enable openvpn` pour qu'OpenVPN démarre automatiquement avec la machine.
!!!

---

### :eight_pointed_black_star: Connexion au serveur OpenVPN

```
openvpn --config /etc/openvpn/client.conf
```

!!! success
Si votre configuration est correcte, vous verrez en dernière ligne:

```
Initialization Sequence Completed
```
!!!
