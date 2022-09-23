---
description: Mise en place et configuration basique d'un serveur DNS Bind9.
categories: [GNU/Linux]
date: 2021-10-20
tags: [GNU/Linux, debian, dns]
visibility: public
order: 48
---

# :hash: Créer un serveur DNS sur debian

![](images/dnsdebian.webp)

==- :icon-report: Mises en garde

!!! Warning Cette documentation est en cours d'écriture :writing_hand:
Il pourrait il y a avoir quelques erreures.  
Si vous en remarquez une, contactez-moi [ici](mailto:contactit.yarka@slmail.me) :slightly_smiling_face:
!!!

!!! primary
Vous pouvez directement utiliser la barre de recherche.  
Ou l'onglet de droite pour trouver la section qui vous intéresse.
!!!

!!! danger
Toutes les adresses ip et masques de cette documentation, sont proposés à titre d'exemple, et peuvent donc, ne pas coïncider avec votre configuration.  
Faites attention, à bien renseigner les vôtres :slightly_smiling_face:
!!!

==-

:information_source: Le serveur DNS s'occupe de traduire une adresse ip en nom de domaine.  

Grâce à celui-ci, je peux par exemple joindre mon serveur nas: `172.16.20.20` avec `nas.it.fr`  

:arrow_right: Plutôt que de retenir une adresse ip, on retient un nom.  

==- :icon-unverified: Un problème avec sudo ?

!!!
Si vous configurez votre serveur directement en root. N'oubliez pas de retirer le `sudo` de chaque commande.
!!!

!!! danger
:arrows_counterclockwise: Si vous avez mis un mot de passe au compte root, la commande `sudo` ne sera pas acceptée.
Connectez-vous directement en root pour exécuter les commandes.  
:arrow_right: Vous pouvez aussi, réinstaller votre système en laissant le mot de passe root vide, lors de l'installation.  
`sudo` s'installera et fonctionnera correctement.
!!!

==-

---

:information_source: Voici la configuration de ce tutoriel:

||| IP du serveur DNS
172.16.10.10
||| Masque Réseau
255.255.0.0
||| Nom de la machine (hostname)
dns
||| Nom de domaine
it.fr
|||

:icon-filter: Ces 4 champs seront à remplacer tout au long du tutoriel par les vôtres (qui correspondent à votre configuration).

---

## :hash: Nommer la machine

```
sudo nano /etc/hostname
```

``` /etc/hostname
dns
```

:arrow_right: Ici on nomme la machine `dns`

---

## :heavy_check_mark: S'assurer que l'adresse IP du serveur est FIXE

```
ip a 
```

``` ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: ens192: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 00:0c:29:cd:01:1a brd ff:ff:ff:ff:ff:ff
    altname enp11s0
    inet 172.16.10.10/16 brd 172.16.255.255 scope global ens192
       valid_lft forever preferred_lft forever
    inet6 fe80::20c:29ff:fecd:11a/64 scope link
       valid_lft forever preferred_lft forever
```

:arrow_right: `ip a` affiche la configuration réseau des interfaces connectées à la machine.

Mon interface `ens192` à pour adresse ip et masque `172.16.10.10/16`  

Si vous avez fixé l'adresse de la machine lors de l'installation (configuration mannuelle du réseau), passez à l'étape suivante.    

==- :arrow_right: Passer l'adresse IP de l'interface en statique

```
sudo nano /etc/network/interfaces
```

``` /etc/network/interfaces

# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface
allow-hotplug ens192
iface ens192 inet static
address 172.16.10.10
netmask 255.255.0.0
gateway 172.16.1.1
```

==-

---

## :memo: Éditer le fichier host

```
sudo nano /etc/hosts
```

``` /etc/hosts
172.16.10.10	dns.it.fr	dns
127.0.0.1 dns
```

`IP_SERVEUR_DNS	HOSTNAME.DOMAINE	HOSTNAME`  
`127.0.0.1 HOSTNAME`

---

## :memo: Éditer le fichier resolv.conf

```
sudo nano /etc/resolv.conf
```

``` /etc/resolv.conf
domain it.fr
search it.fr
nameserver 172.16.10.10
```

!!! danger
Il est nécessaire de redémarrer la machine:

```
sudo reboot
```
!!!

:arrow_right: Après redémarrage de la machine, passez à l'étape suivante.

---

## :arrow_heading_down: Installez bind9

```
sudo apt update  &&  sudo apt install bind9 dnsutils
```

:icon-chevron-right: `sudo apt update` va mettre à jour la liste des paquets en se basant sur le fichier `sources.list`  

:icon-chevron-right: `sudo apt install bind9 dnsutils` installe bind9 pour gérer les zones DNS par la suite.  

---

## :memo: Copier et renommer la template de configuration

```
sudo cp /etc/bind/db.local /etc/bind/db.it.fr
```

:information_source: La commande `cp` nous permet de copier db.local (le fichier de configuration par défaut de bind9), et de le renommer en un nouveau fichier `db.it.fr`

---

## :memo: Éditer le fichier de configuration des zones DNS

!!! success
Pour gagner du temps, on va directement remplacer les champs "localhost" par "it.fr" (notre domaine), dans le fichier de configuration.
!!!

:point_right: Pour ce faire nous utilisons l'utilitaire sed:

```
sudo sed 'i/localhost/it.fr/g' db.it.fr
```

:point_right: Vérifiez votre configuration:

``` db.it.fr
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     it.fr. root.it.fr. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      localhost.
@       IN      A       172.16.10.10
@       IN      AAAA       ::1
dns       IN       A       172.16.10.10
client       IN       A       172.16.20.20
```

!!!
L'enregistreament `A` nommé `client` nous permet d'atteindre `172.16.20.20` avec `client.it.fr`
!!!

==- :icon-diff-added: Rajouter un enregistrement DNS :  

||| Hostname
nas
||| IN
IN
||| Type
A
||| Adresse IP
172.16.30.30
|||

``` db.it.fr
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     it.fr. root.it.fr. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      localhost.
@       IN      A       172.16.10.10
@       IN      AAAA       ::1
dns       IN       A       172.16.10.10
client       IN       A       172.16.20.20
nas       IN       A       172.16.30.30
```

==-

:icon-note: Voici une description des principaux types d'enregistrements DNS:

A | AAAA | CNAME | MX | TXT | NS | SOA | SRV | PTR 
--- | --- | --- | --- | --- | --- | --- | --- | ---
Associe un nom d’hôte à une adresse ipv4 (32 bits) | Associe un nom d’hôte à une adresse ipv6 (128 bits) | Transfère un domaine ou un sous-domaine à un autre domaine, ne fournit PAS d'adresse IP | Dirige le courrier vers un serveur de messagerie | Peux servir a enregistrer des notes. Il est souvent utilisé pour la sécurité mails. | Stocke le serveur de noms pour une entrée DNS | Stocke les informations administratives d'un domaine | Spécifie un port pour des services spécifiques | Fournit un nom de domaine dans les recherches inversées. La résolution inverse (le contraire du type A).

[!ref target="blank" text="Liste complète"]( https://www.cloudflare.com/fr-fr/learning/dns/dns-records/)

---

## :memo: Éditer le fichier named.conf

:point_right: Il est nécessaire de spécifier le chemin des fichiers de configurations des zones DNS:

```
sudo nano /etc/bind/named.conf.local
```

``` # /etc/bind/named.conf.local
//
// Do any local configuration here
//

// Consider adding the 1918 zones here, if they are not used in your
// organization
//include "/etc/bind/zones.rfc1918";

zone "it.fr" {
        type master;
        file "/etc/bind/db.it.fr";
        allow-query { any; };
};
zone "10.16.172.in-addr.arpa" {
        type master;
        file "/etc/bind/db.it.fr.inv";
};
```

:icon-chevron-right: `Ligne 9` `zone "MON_DOMAIN"`  

:icon-chevron-right: `Ligne 11` `file "/etc/bind/db.MON_DOMAIN";`  

:icon-chevron-right: `Ligne 14` Adresse inversée: `zone "3_PREMIERS_OCTETS_ADDRESSE_RESEAU.in-addr.arpa"`  

Exemple: Si mon adresse réseau est: `192.168.1.0/24` inversée: `1.168.192`  

:icon-chevron-right: `Ligne 16` `file "/etc/bind/db.MON_DOMAIN.inv";`  

---

## :memo: Éditer le fichier named.conf.options

:icon-repo-push: Nous allons maitenant configurer le fichier qui gère les options de redirection des requêtes:
```
sudo nano /etc/bind/named.conf.options
```

``` # /etc/bind/named.conf.options
options {
	directory "/var/cache/bind";

	// If there is a firewall between you and nameservers you want
	// to talk to, you may need to fix the firewall to allow multiple
	// ports to talk.  See http://www.kb.cert.org/vuls/id/800113

	// If your ISP provided one or more IP addresses for stable 
	// nameservers, you probably want to use them as forwarders.  
	// Uncomment the following block, and insert the addresses replacing 
	// the all-0's placeholder.

	 forwarders {
	 	172.16.10.10;
	 	1.1.1.1;
	 };

	//========================================================================
	// If BIND logs error messages about the root key being expired,
	// you will need to update your keys.  See https://www.isc.org/bind-keys
	//========================================================================
	dnssec-validation auto;

	auth-nxdomain no;    # conform to RFC1035
	version none;
        	forward only;
	// listen-on-v6 { any; };
};
```

:point_right: Ligne 13, l'option `forwaders` définie les serveurs DNS.  
Je renseigne donc l'adresse IP de mon serveur DNS.  

:point_right: C'est aussi grâce à elle, que les machines du réseau peuvent sortir sur le WAN,  
en spécifiant un DNS public (`cloudflare: 1.1.1.1` ou `google: 8.8.8.8` etc).










