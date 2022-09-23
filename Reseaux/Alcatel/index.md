---
title: Configuration d'un switch Alcatel
icon: three-bars
description: Commandes utiles pour switch Alcatel.
categories: [Reseaux]
date: 2021-11-12
tags: [alcatel, networks]
visibility: public
order: 99
---

# :gloves: Configuration d'un switch Alcatel

---

## :bookmark_tabs: Afficher la configuration:  

```
show configuration snapshot all
```

---

## :memo: Sauvegarder la configuration:

```
write memory flash-synchro
```

---

## :scissors: Supprimer la configuration:  

```
rm /flash/working/boot.cfg
rm /flash/certified/boot.cfg
cd working
reload working no rollback-timeout
```

Confirmer avec `Y` pour "Yes".  

---

## :clipboard: Addresser une interface

```
ip interface <NAME> address <@IP> mask <MASK> vlan <VLAN_NUM>
```

- Lister les interfaces  

```
show ip interface
```

---

## :card_index_dividers: Gestion des Vlans

---

### :file_folder: Création d'un vlan:  

```
vlan 10 name test
```

Création du vlan 10 avec comme nom `test`

---

### :x: Suppression d'un vlan:  

```
no vlan 10
```

Supprime le vlan 10

---

### :heavy_minus_sign: Voir la repartition des vlans sur les ports

```
show vlan port
```

---

## :wavy_dash: Configuration du stack

---

### :wavy_dash: Modifier le slot du stack:  

```
stack set slot 1 saved-slot 2
```

`1` est le numéro actuel du slot  
`2` est le numéro par lequel je veux le remplacer  

---

### :wavy_dash: Visualiser la topologie du stack:

```
show stack topology
```

---

## :sparkle: Configuration SNMP

---

### :ballot_box_with_check: Créer un utilisateur:  

```
user user1 read-only all password test
```

Création de l'utilisateur `user1` avec comme mot de passe `test`  

---

### :beginner: Configuration du serveur SNMP

```
snmp security no security
```

---

### :beginner: Association de l'utilisateur à la communauté SNMP:  

```
snmp community map mycommunity user user1 only
```

Association de l'utilisateur `user1` à la communauté `mycommunity`


---

## :curly_loop: POE

```
lanpower start 1/1/1
```

---

## :eight_pointed_black_star: Changer le mot de passeadmin

```
user admin password
```