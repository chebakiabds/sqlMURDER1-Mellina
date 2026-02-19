
#  SQL Murder Mystery – Rapport d’enquête
**Nom  :** [BENDRIS Mellina] 
**Date :** 17 janvier 2026
**Projet :** SQL Murder Mystery – qui est le meurtrier ?

---

## 1️ Introduction

Ce rapport décrit toutes les étapes et les lignes de code que j'ai tapé pour identifier le meurtrier dans la base de données `sql-murder-mystery`.  


======================================================================
(pour certaine commandej'ai fait un LIMIT 3 en exemple pour montrer le resultat comme ca c'est epurée)
## 2️ Ouverture de la base

```sql
sqlite3 ma_base.db
```
3.  LES TABLES

Commande utilisée :
```sql
.tables
.schema person;
SELECT * FROM person;
ou .tables (jai fait les deux)
```

crime_scene_report      get_fit_now_check_in    interview
drivers_license         get_fit_now_member      person
facebook_event_checkin  income                  solution

Cette table contient toutes les personnes qui pourraient être soit des suspects ou témoins.
Il faut regarder les colonnes pour savoir comment relier chaque personne à ses caractéristiques et son permis.




Dans ces tables il ya toutes les infos necessaires pour identifier le meurtrier .
Je pensais au départ que certaines tables n’existaient pas car je ne connaissais pas leurs noms exacts.

La commande .tables m’a permis de voir toutes les tables.


======================================================================

4. EXPLORATION DES TABLES

----------------------------------------------------------------------
4.1 TABLE crime_scene_report
----------------------------------------------------------------------

Commande utilisée :
```SQL
.schema crime_scene_report
SELECT * FROM crime_scene_report
```

Erreur rencontrée :
La requête suivante ne retournait aucun résultat :
```SQL
SELECT *
FROM crime_scene_report
WHERE type = 'murder' AND date = '2018-01-15'
```

Raison de l’erreur :
Le format exact de la date ne correspondait pas aux données stockées dcp jai cherché dautre commande a executé pour avoir les infos.
jai mis ca :

 ```SQL
SELECT *
FROM crime_scene_report
WHERE type LIKE '%murder%'
```
Voici ce qui saffiche apres avoir mis cette commande :

| Date     | Type  | Description                                         | City       |
|----------|--------|-----------------------------------------------------|-----------|
| 20180115 | murder | Life? Dont talk to me about life.                   | Albany    |
| 20180115 | murder | Mama, I killed a man, put a gun against his head... | Reno      |
| 20180215 | murder | REDACTED REDACTED REDACTED                          | SQL City  |


Explication :
Jai utilisé LIKE pour élargir la recherche et obtenir les infos du crime autour du 15 janvier 2018.(comme vous nous l'avez indiqué j'ai cherché par rapport à ça.)

----------------------------------------------------------------------
4.2 TABLE person
----------------------------------------------------------------------

Commande utilisée :
```SQL
.schema person
SELECT * FROM person
```

Erreur rencontrée :
Au début, je ne savais pas quelle table contenait les informations sur les personnes.

Après exploration, j’ai identifié que la table person regroupait tout les suspect et les temoins.
Cette table contient toutes les personnes enregistrées dans la base.

----------------------------------------------------------------------
4.3 TABLE interview
----------------------------------------------------------------------

Commande utilisée :
```SQL
.schema interview
SELECT * FROM interview
```

| ID    | Texte                                      |
|-------|--------------------------------------------|
| 28508 | ‘I deny it!’ said the March Hare.           |
| 63713 |                                            |
| 86208 | way, and the whole party swam to the shore. |



J’ai essayé de filtrer par date, mais la colonne n’existe pas.
donc
J’ai affiché tous les témoignages afin d’identifier ceux qui sont liés au meurtre. (voici afficher les trois premieres lignes de codes )
Cette table contient les témoignages des témoins et fournit des indices essentiels.

----------------------------------------------------------------------
4.4 TABLE drivers_license
----------------------------------------------------------------------

Commande utilisée :
```SQL
.schema drivers_license
SELECT * FROM drivers_license
```

| ID     | Âge | Taille | Couleur cheveux | Couleur yeux | Sexe   | Plaque | Marque   | Modèle        |
|--------|-----|--------|-----------------|--------------|--------|--------|----------|---------------|
| 188785 | 27  | 50     | amber           | blue         | male   | 036ND7 | Buick    | Lucerne       |
| 188814 | 63  | 50     | blue            | white        | male   | 1JH522 | Honda    | Civic         |
| 189131 | 89  | 79     | green           | white        | female | 74T56J | Kia      | Optima        |
| 189149 | 66  | 74     | blue            | white        | female | 606JT8 | Isuzu    | i-280         |
| 189336 | 48  | 58     | amber           | green        | female | 1H2QPA | Nissan   | Sentra        |
| 189545 | 41  | 70     | blue            | grey         | male   | 365I42 | GMC      | 3500          |
| 189658 | 66  | 72     | blue            | white        | female | UDCLEO | Lexus    | GX            |
| 189671 | 71  | 61     | brown           | green        | female | IH52F7 | Pontiac | Trans Sport   |
| 189720 | 70  | 71     | black           | black        | female | 4M64A0 | Audi     | S8            |
| 189766 | 25  | 57     | black           | white        | female | 36LGS4 | Infiniti | M             |



J’ai d’abord cherché les plaques d’immatriculation dans la table person.

Après vérification, j’ai compris que les plaques sont stockées dans drivers_license.

Cette table contient les caractéristiques physiques et les plaques d’immatriculation.

----------------------------------------------------------------------
4.5 TABLE get_fit_now_member
----------------------------------------------------------------------

Commande utilisée :`
```SQL
.schema get_fit_now_member
SELECT * FROM get_fit_now_member
```


| ID     | Âge | Taille | Couleur cheveux | Couleur yeux | Sexe   | Plaque | Marque   | Modèle        |
|--------|-----|--------|-----------------|--------------|--------|--------|----------|---------------|
| 207789 | 86  | 75     | black           | blonde       | male   | 7HINPJ | Suzuki   | Reno          |
| 207790 | 54  | 81     | brown           | white        | male   | WF7AR1 | Infiniti | QX            |
| 207830 | 30  | 76     | amber           | brown        | female | 3P5L6R | Toyota   | Land Cruiser  |




Confusion avec la table get_fit_now_check_in.
dcp jai fait ça :
```SQL
- get_fit_now_member : informations sur l’inscription
```
voici ce qui apparait apres entrée

| Code  | ID    | Nom complet          | Date     | Statut  |
|-------|-------|----------------------|----------|---------|
| NL318 | 65076 | Everette Koepke      | 20170926 | gold    |
| AOE21 | 39426 | Noe Locascio         | 20171005 | regular |
| 2PN28 | 63823 | Jeromy Heitschmidt   | 20180215 | silver  |

```SQL
- get_fit_now_check_in : informations sur les passages à la salle
```
pareil avec cette commande:

| Code  | Date     | Valeur 1 | Valeur 2 |
|-------|----------|----------|----------|
| NL318 | 20180212 | 329      | 365      |
| NL318 | 20170811 | 469      | 920      |
| NL318 | 20180429 | 506      | 554      |


----------------------------------------------------------------------
4.6 TABLE get_fit_now_check_in
----------------------------------------------------------------------

Commande utilisée :
```SQL
.schema get_fit_now_check_in
SELECT *
FROM get_fit_now_check_in
WHERE check_in_time LIKE '2018-01-15%'
```

  membership_id text,
        check_in_date integer,
        check_in_time integer,
        check_out_time integer,
        FOREIGN KEY (membership_id) REFERENCES get_fit_now_member(id)



Utilisation de la colonne check_in_date qui n’existe pas.
Après consultation du schéma, j’ai utilisé la colonne check_in_time.

----------------------------------------------------------------------
4.7 TABLE facebook_event_checkin
----------------------------------------------------------------------

Commande utilisée :
```SQL
.schema facebook_event_checkin
SELECT *
FROM facebook_event_checkin
WHERE check_in_time LIKE '2018-01-15%'
```

Erreur rencontrée :
Utilisation de la colonne event_date inexistante.
erreur pour le FROM facebook_event_checkin;  car j'ai oublie le SELECT * 


Correction avec la colonne check_in_time.

----------------------------------------------------------------------
4.8 TABLE income
----------------------------------------------------------------------

Commande utilisée :
```SQL
.schema income
SELECT * FROM income
```

Explication :
Cette table permet de compléter le profil des personnes mais n’est pas déterminante.

----------------------------------------------------------------------
4.9 TABLE solution
----------------------------------------------------------------------

Commande utilisée :
```SQL
SELECT * FROM solution
```

Erreur rencontrée :
Cette table a été consultée trop tôt.

Solution :
Elle a finalement été utilisée uniquement pour vérifier la réponse finale.

======================================================================

5. RECOUPEMENT DES INFORMATIONS

Commande utilisée :
```SQL
SELECT *
FROM person p
JOIN drivers_license d
ON p.license_id = d.id
```

| ID    | Nom complet           | N° dossier | N° rue | Rue              | Téléphone | SSN    | Âge | Taille | Cheveux | Yeux  | Sexe   | Plaque | Marque          | Modèle          |
|-------|-----------------------|------------|--------|------------------|-----------|--------|-----|--------|---------|-------|--------|--------|------------------|-----------------|
| 10000 | Christoper Peteuil    | 993845     | 624    | Bankhall Ave     | 747714076 | 993845 | 46  | 59     | black   | green | male   | 557472 | Chrysler         | Town & Country  |
| 10007 | Kourtney Calderwood   | 861794     | 2791   | Gustavus Blvd    | 477972044 | 861794 | 54  | 74     | black   | white | female | 3P6DMS | BMW              | M Roadster      |
| 10010 | Muoi Cary             | 385336     | 741    | Northwestern Dr  | 828638512 | 385336 | 24  | 79     | blue    | green | female | GM6Y5J | Mercedes-Benz    | CLS-Class       |




Erreur rencontrée :
Au départ, je consultais les tables séparément.


Le JOIN m’a permis de croiser les informations et de filtrer les suspects plus efficacement.

======================================================================

6. COMMANDE POUR TROUVER LE CRIME EXACT SELON LA DATE
``` sql
SELECT *
FROM crime_scene_report
WHERE type LIKE '%murder%'
AND date LIKE '2018%';
```



Solution obtenue :
Le meurtrier identifié est Jeremy Bowers.

======================================================================




LIRE LES TEMOIGNAGES
``` SQL
SELECT *
FROM interview
WHERE transcript LIKE '%gym%';
```

14887|I heard a gunshot and then saw a man run out. He had a "Get Fit Now Gym" bag. The membership number on the bag started with "48Z". Only gold members have those bags. The man got into a car with a plate that included "H42W".
16371|I saw the murder happen, and I recognized the killer from my gym when I was working out last week on January the 9th.
sqlite>


LES PASSAGES A LA SALLE DEBUT JANVIER 2018
j'ai tapé ce code :
``` SQL
SELECT *
FROM get_fit_now_check_in
WHERE check_in_time LIKE '2018-01%';
```
sauf que rien n'est apparu donc j'ai utilisé une intervalle de date :
``` SQL
SELECT *
FROM get_fit_now_check_in
WHERE check_in_time >= '2018-01-01'
  AND check_in_time < '2018-02-01';
```

mais toujouts rien donc j'ai dabord verifier si il y'avait bien des données dans cette table au cas ou avec cette commande :
``` SQL
SELECT * FROM get_fit_now_check_in;
```

il y'en avait bien donc verifier le format reel de check_in_time :
 ``` SQL
SELECT check_in_time
FROM get_fit_now_check_in
LIMIT 5;
```

j'ai eu ce resultat :
329
469
506
124
418

Ensuite j'ai fait un pragma pour afficher la structure :

``` SQL
PRAGMA table_info(get_fit_now_check_in);
```

j'ai donc ca qui apparait : 

0|membership_id|TEXT|0||0 1|check_in_date|INTEGER|0||0 2|check_in_time|INTEGER|0||0 3|check_out_time|INTEGER|0||0 

de ce que j'ai compris vu que les dates sont en INTEGER c'est pas des dates au formats texte c'etait pour ca que le LIKE  ne renvoyer rien


apres j'ai chercher tout les check_ins de janvier 2018 :

``` SQL
SELECT *
FROM get_fit_now_check_in
WHERE check_in_date BETWEEN 20180101 AND 20180131;
```

et j'ai tapé ca dans le cas ou je cherche une date precise :

``` SQL
SELECT *
FROM get_fit_now_check_in
WHERE check_in_date = 20180115;
```
 j'ai bien les infos

| membership_id | check_in_date | check_in_time | check_out_time |
| ------------- | ------------- | ------------- | -------------- |
| D2KY6         | 20180115      | 746           | 836            |
| 344VM         | 20180115      | 1087          | 1195           |
| 3BRSC         | 20180115      | 354           | 825            |
| HM6U8         | 20180115      | 525           | 800            |




RELIER UNE PLAQUE A UNE PERSONNE
``` SQL
SELECT p.name, d.plate_number
FROM person p
JOIN drivers_license d
ON p.license_id = d.id
WHERE d.plate_number LIKE '%H42W%';
```

croiser salle de sport et la plaque 

``` SQL
SELECT p.name, m.id
FROM get_fit_now_member m
JOIN person p
ON m.person_id = p.id
WHERE m.id LIKE '48Z%';
```

Tomas Baisley|48Z38
Joe Germuska|48Z7A
Jeremy Bowers|48Z55

donc la teoriquement j'ai le choix entre ces trois personnes



Voici la requete finale pour trouver le meurtrier :
```sql
SELECT p.name
FROM get_fit_now_check_in AS c
JOIN get_fit_now_member AS m
  ON c.membership_id = m.id
JOIN person AS p
  ON m.person_id = p.id
JOIN drivers_license AS dl
  ON p.license_id = dl.id
WHERE
  m.membership_status = 'gold'
  AND m.id LIKE '48Z%'
  AND c.check_in_date = '20180109'
  AND dl.plate_number LIKE '%H42W%'
LIMIT 1;
```
7. CONCLUSION

Conclusion :
Malgré plusieurs erreurs au début (mauvaises colonnes, filtres incorrects, confusion entre certaines tables, j'ai galerer aussi sur la structure des date mais apres ca aller),dcp avec la correction de chaque code que j'ai raté j'ai réussi a trouver le meurtrier._



































