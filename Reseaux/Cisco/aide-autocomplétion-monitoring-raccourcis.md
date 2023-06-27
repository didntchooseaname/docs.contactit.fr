---
description: Informations et tips relatifs au CLI CISCO.
categories: [Réseaux, Cisco]
date: 2022-09-14
tags: [Networks, config, tuto, Cisco]
order: 48
---

# :mag_right: Aide, autocomplétion, monitoring, raccourcis  

---

## :interrobang: Aide  

L'option qui permet d'afficher les options disponibles d'une commande est `?`

```
Router#show ?
  aaa                Show AAA values
  access-lists       List access lists
  arp                Arp table
  cdp                CDP information
  class-map          Show QoS Class Map
  clock              Display the system clock
  controllers        Interface controllers status
  crypto             Encryption module
  debugging          State of each debugging option
  dhcp               Dynamic Host Configuration Protocol status
  dot11              IEEE 802.11 show information
  file               Show filesystem information
  flash:             display information about flash: file system
  flow               Flow information
  frame-relay        Frame-Relay information
  history            Display the session command history
  hosts              IP domain-name, lookup style, nameservers, and host table
  interfaces         Interface status and configuration
  ip                 IP information
  ipv6               IPv6 information
  license            Show license information
  line               TTY line information
 --More-- 
```

:top: Ici le `?` montre les commandes possibles avec **show**.  
- le `--More--` indique qu'il y a trop de commandes pour la fenêtre d'affichage.  
- La touche `Entrer` permet de faire défiler ligne par ligne.  
- La touche `Espace` permet de faire défiler bloc par bloc.  

---

## :wavy_dash: L'Autocomplétion

Les commandes s’autocomplètent avec la touche de tabulation.  
Voici quelques exemples:
- `sh <tabulation>` devient `show`
- `int` devient `interface`

Vous pouvez commencer à tapper la commande et faire TAB dès que possible.Il se peut qu'il y ait une ambiguïté.
Il faut donc continuer à tapper la commande, pour préciser au système la commande en question.
Pour qu'il puisse la compléter correctement.

---

##  🗃️ Filtrage des résultats

:icon-chevron-right: Ajouter `|` après une commande permet de filtrer le résultat de cette dernière:  

``` Router#sh ru | include interface
interface GigabitEthernet0/0/0
interface GigabitEthernet0/0/1
interface GigabitEthernet0/0/2
interface Vlan1
```

:icon-chevron-right: Ici `include` filtre le résultat qui inclus le mot `interface`.

---

## :black_left_pointing_double_triangle_with_vertical_bar: Les commandes abrégées

Le CLI Cisco prends en charge les abréviations des commandes.  
Voici quelques exemples:

 Abréviation  | Commande complète
---   | ---
conf t | configure terminal
sh ip int b | show interface brief
wr | Write memory
int g0/0 | interface GigabitEthernet0/0

---

## :x: Le signalement d'érreures

L’environnement indique l’endroit ou il y a une erreur.  
Syntaxe incorrecte ou mauvais mode (do « commande »).

```
Router#sh ip routte
                 ^
% Invalid input detected at '^' marker.
```
 
---

## :symbols: Les Raccourcis clavier  

 Séquence  | Description
---   | ---
CTRL-A| Début de ligne
CTRL-E | Fin de ligne
CTRL-P ou bas | Commande précédente
CTRL-N ou haut | Commande suivante
CTRL-F ou droite | Curseur vers la droite
CTRL-B ou gauche | Curseur vers la gauche
CTRL-Z | Retour au mode privilége
CRTL-C | Interruption
CTRL-SHIFT-6 / CTRL-SHIFTS | Interruption forcée
TAB | Compléte une commande
Backspace | Efface un caractére vers la gauche
CTRL-R | Réaffichage de la ligne 
CTRL-U | Efface la ligne
CTRL-W | Efface un mot 
ESC-b | recule d'un mot
ESC-f | Avance d'un mot
 
---

## :beginner: Quelques astuces

La commande `do` permet d’exécuter une commande du mode privilège dans un autre mode.

```
Switch(config)#show run
                 ^
% Invalid input detected at '^' marker.

Switch(config)#do show run
Building configuration...
```

---

## :name_badge: Désactivation des recherches DNS

Avec résolution DNS (présente par défaut), une frappe erronée ne correspondant pas à une commande peut être interprétée, en mode privilège, comme une tentative de connexion distante SSH et bloquer un certain temps la console :

```
Switch#showe
Translating "showe"...domain server (255.255.255.255)
```

Pour désactiver la résolution DNS:

``` Switch(config)#
no ip domain-lookup
```

---

## :name_badge: Désactivation des annonces

À chaque fois que vous changez l'état d'une interface (allumer(up), éteindre(down), etc...)  
L'équipement écrit un message dans le CLI, vous indiquant ce qui a changé.
Ce qui peut être dérangeant, lorsque l'on écrit des commandes.
Il est possible désactiver ses annonces:

```
%LINK-1-CHANGED: Interface FastEthernet0/1, changed state to up
```

``` Switch(config)#
no logging console
```
 
---

## :new: Renommer un équipement

``` Switch(config)#
hostname MON_SWITCH
```

---

## :keycap_star: Définir un mot de passe enable

Pour rentrer dans le mode "enable" de l'équipement, il est possible et même recommandé, de mettre un mot de passe.

```
Switch(config)#enable secret MON_MDP
```

Grâce à l'option `secret` le mot de passe est hashé.

---

## :o: Voir la configuration générale

Pour afficher les informations sur la configuration globale (hostname, services, interfaces, spanning-tree, mot de passe, etc…), il faut exécuter cette commande:

```
Switch#show running-config
Building configuration...

Current configuration : 1080 bytes
!
version 15.0
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption
!
hostname Switch
!
!
!
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
interface FastEthernet0/1
!
interface FastEthernet0/2
--More--
```

---

## :o: Voir la configuration des vlans

Pour afficher les informations sur la configuration des vlans: 

```
Switch#show vlan

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/1, Fa0/2, Fa0/3, Fa0/4
                                                Fa0/5, Fa0/6, Fa0/7, Fa0/8
                                                Fa0/9, Fa0/10, Fa0/11, Fa0/12
                                                Fa0/13, Fa0/14, Fa0/15, Fa0/16
                                                Fa0/17, Fa0/18, Fa0/19, Fa0/20
                                                Fa0/21, Fa0/22, Fa0/23, Fa0/24
                                                Gig0/1, Gig0/2
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    

VLAN Type  SAID       MTU   Parent RingNo BridgeNo Stp  BrdgMode Trans1 Trans2
---- ----- ---------- ----- ------ ------ -------- ---- -------- ------ ------
1    enet  100001     1500  -      -      -        -    -        0      0
1002 fddi  101002     1500  -      -      -        -    -        0      0   
1003 tr    101003     1500  -      -      -        -    -        0      0   
1004 fdnet 101004     1500  -      -      -        ieee -        0      0   
1005 trnet 101005     1500  -      -      -        ibm  -        0      0   
 --More-- 
```

!!! primary
Les vlans 1002, 1003, 1004, 1005 sont réservés dans le système pour du token ring.
Ce système étant obsolète ces vlans ne sont pas utilisés.
!!!

:information_source: Ici les interfaces F0/1-24 et G0/1-2 appartiennent au vlan 1.

---

## :x: Supprimer/enlever/désactiver un élément

Le suffixe `no` permet de retirer/désactiver/supprimer, n’importe quelle commande ou configuration précédemment écrite.

``` Router(config-if)#
no ip address 192.168.1.1 255.255.255.0
```

:exclamation: Retire l’adresse ip assignée à l’interface.

```
Router(config)#no router ospf
```

:exclamation: Désactive le protocole OSPF (et sa configuration) du routeur.

---

:icon-diff-renamed: Dernière modification: 14/09/2022 - 12h00

