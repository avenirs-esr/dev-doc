---
layout: page
title: LDAP Data source
permalink: /ldap-data-source/
page_content_classes: table-container
up: ../data-provisioning/
---

<br/>
**Projet :** Avenirs-ESR / ePortfolio. <br/>
**Objet :** Alimentation depuis un annuaire LDAP au format Supann.<br/>
<br/>
**Révision :** 1.0.0<br/>
**Date :** 15/01/2026<br/>
**Auteur :** A. Deman<br/>
**Commentaire :** Version initiale<br/>

## Contexte

- L'objectif est d'étudier l'alimentation, pour les étudiants dans un premier temps, a partir d'un annuaire proche de ceux des établissements.
- La plupart des établissements respectent intégralement ou partiellement le format [Supann](https://services.renater.fr/documentation/supann/courant/recommandations/index){:target="_blank"}.
- Un export au format json est réalisé à partir d'un annuaire LDAP puis traité pour être intégré dans le système.

**Jeux de données :** [export JSON de 4000 entrées](../assets/provisioning/students.json){:target="_blank"}.

## Remarques
- Aucune garantie du respect de la norme Supann dans les établissements.
- Possibilité de mélange de plusieurs versions de la norme avec la présence d'attributs obsolètes.
- Certains formats sont étiquetés et pourraient potentiellement être enrichis via des API open data.
    - [Annuaire de l'éducation nationale](https://www.data.gouv.fr/dataservices/annuaire-de-leducation-nationale){:target="_blank"}
    - [SISE](https://data.esr.gouv.fr/FR/M411/sise){:target="_blank"}
- Concernant l'INE, il est possible que la lettre à la fin (Clé) soit omise ainsi que les 0 de préfixe, ce qui rend difficile son utilisation.

## Attributs

| LDAP | Multivalué | JSON | Base de données<br/>bd.schema.table.colone |Définition | Exemple de valeur | Remarques |
|------|------|----------------|-------------|-------------------|-----------|
| uid | Multivalué, mais mono-valuation demandée dans le cadre de SupAnn |externalId    |   |[UID](https://services.renater.fr/documentation/supann/courant/recommandations/attributs/uid){:target="_blank"}, Identifiant de connexion d'une personnes sur les systèmes informatiques| akamo | Pourrait être utilisé pour external_id (à la place d'EduPersonPrincipalName) |
| cn |Oui, mais ne devrait contenir qu'une valeur  |    |   |[CommonName](https://services.renater.fr/documentation/supann/courant/recommandations/attributs/cn){:target="_blank"}, Nom complet sans accent d'une personne, identifiant d'un groupe| ALLAN AKAMO |  |
| givenName | Oui, mais ne devrait contenir qu'une valeur | firstName |avenirs_api.dev.external_user.first_name | [GivenName](https://services.renater.fr/documentation/supann/courant/recommandations/attributs/givenname){:target="_blank"}, Prénom d'usage d'une personne| ALLAN | La création d'un external_user doit déclencher la création d'un enregistrement dans les tables user et student |
| displayName | Non  | displayName | |[DisplayName](https://services.renater.fr/documentation/supann/courant/recommandations/attributs/displayname){:target="_blank"}, Nom complet d'affichage (peut contenir des accents)     | AKAMO ALLAN |  |
| mail | Multivalué, mais mono-valuation demandée dans le cadre de SupAnn | mail |avenirs_api.dev.external_user.email  |[Mail](https://services.renater.fr/documentation/supann/courant/recommandations/attributs/mail){:target="_blank"}  | akamo@etab.fr  |  |
| eduPersonPrimaryAffiliation | Non |  |avenirs_api.dev.external_user.category   |[EduPersonPrimaryAffiliation](https://services.renater.fr/documentation/supann/courant/recommandations/attributs/edupersonprimaryaffiliation){:target="_blank"},Statut principal de la personne vis-à-vis de l'établissement  | student |  |
| sn | Oui, mais ne devrait contenir qu'une valeur |lastName  | avenirs_api.dev.external_user.last_name | [Surname](https://services.renater.fr/documentation/supann/courant/recommandations/attributs/sn){:target="_blank"},Nom d'usage de la personne (nom de famille)| AKAMO |  |
| supannEtablissement |Oui  | structure | Refactoring nécessaire pour intégrer UAI: avenirs_api.dev.institution<br/> | [SupannEtablissement](https://services.renater.fr/documentation/supann/courant/recommandations/attributs/supannetablissement){:target="_blank"},Code d'un établissement (UAI)| 0123456G |  |
| supannCivilite | Non  | civility |  | [SupannCivilite](https://services.renater.fr/documentation/supann/courant/recommandations/attributs/supanncivilite){:target="_blank"},Civilité | 2 valeurs possibles: M. ou Mme  |  |
| supannOIDCDateDeNaissance |Non  |birthDate | | [SupannOIDCDateDeNaissance](https://services.renater.fr/documentation/supann/courant/recommandations/attributs/supannOIDCDateDeNaissance){:target="_blank"},Date de naissance au format AAAA-MM-JJ | 1999-11-01 |  |
| supannEntiteAffectationPrincipale | Non| affectationPrincipale |  | [SupannEntiteAffectationPrincipale](https://services.renater.fr/documentation/supann/courant/recommandations/attributs/supannEntiteAffectationPrincipale){:target="_blank"}, Identifiant de l'entité principale d'affectation | ED548 | Le code est interne à l'établissement mais s'il est renseigné, doit correspondre à un code existant dans la branche structure |
| supannEntiteAffectation | Oui | affectations |  | [SupannEntiteAffectation](https://services.renater.fr/documentation/supann/courant/recommandations/attributs/supannEntiteAffectation){:target="_blank"}, Identifiant de l'entité d'affectation | ED548 | Multivalué, peut contenir d'autres codes en plus de celui de l'affectaion principale (Ce sont des codes de structure) |
| eduPersonPrincipalName | Non | eppn | avenirs_api.dev.external_user.external_id  | [EPPN](https://services.renater.fr/documentation/supann/courant/recommandations/attributs/edupersonprincipalname){:target="_blank"}, Identifiant globalement unique pour une personne, composé de deux parties séparées par un “@”: celle de gauche identifie la personne au sein d'un établissement, et celle de droite identifie cet établissement | akamo@etab.fr | L'external_id pourrait également être valué via l'uid |
| supannEtuSecteurDisciplinaire |Oui  |  |  | [SupannEtuSecteurDisciplinaire](https://services.renater.fr/documentation/supann/courant/recommandations/attributs/supannEtuSecteurDisciplinaire){:target="_blank"}, secteur disciplinaire d'un diplôme ou d'une formation  | {SISE}0041 |   Donnée étiquetée. Si l'étiquette est {SISE} voir si elle peut être enrichie via API open data. 
| supannEtuDiplome | Oui | degree |  | [SupannEtuDiplome](https://services.renater.fr/documentation/supann/courant/recommandations/attributs/supannEtuDiplome){:target="_blank"},Diplôme préparé par l'étudiant  | {UAI:0123456G}T3GEA-841 | Format étiqueté, dans l'exemple il s'agit d'un code local |
| supannEtuTypeDiplome | Oui | degreeType |  | [SupannEtuTypeDiplome](https://services.renater.fr/documentation/supann/supann2021/recommandations/attributs/supannetutypediplome){:target="_blank"},type ou catégorie du diplôme préparé | {SISE}DR |Format étiqueté  |
| supannEtuEtape | Oui | step |  | [SupannEtuEtape](https://services.renater.fr/documentation/supann/courant/recommandations/attributs/supannetuetape){:target="_blank"},Code identifiant une étape d'enseignement | {UAI:0123456G}PIFCRO-100 | Format étiqueté |
| supannAffectation | Oui | audience |  |  |  | [Attribut obsolète](https://services.renater.fr/documentation/supann/recommandations2020/attributs/attributs_obsoletes){:target="_blank"}, remplacé par supannEntiteAffectation |
| supannEtuCursusAnnee |Oui  | cursusYear |  | [SupannEtuCursusAnnee](https://services.renater.fr/documentation/supann/courant/recommandations/attributs/supannetucursusannee){:target="_blank"}, type de cursus (L, M, D ou X, …) ainsi que l'année dans le diplôme | {SUPANN}X2 | Valeurs normalisées si étiquette SUPANN. Voir la [définition de l'attribut](https://services.renater.fr/documentation/supann/courant/recommandations/attributs/supannetucursusannee#utilisation){:target="_blank"} pour les valeurs possibles |
| supannNomDeNaissance | Non  |  |  | [SupannNomDeNaissance](https://services.renater.fr/documentation/supann/courant/recommandations/attributs/supannnomdenaissance){:target="_blank"}, Nom de famille de naissance de la personne | AKAMO |  |
| eduPersonAffiliation | Oui |  |  | [EduPersonAffiliation](https://services.renater.fr/documentation/supann/courant/recommandations/attributs/eduPersonAffiliation){:target="_blank"}, Statut principal de la personne vis-à-vis de l'établissement | student | Cet attribut  PEUT suivre la nomenclature précisée dans  la [définition de l'attribut](https://services.renater.fr/documentation/supann/courant/recommandations/attributs/eduPersonAffiliation#utilisation){:target="_blank"} |
| supannCodeINE |Multivalué mais ne devrait contenir qu'une valeur  | INE |  | [SupannCodeINE](https://services.renater.fr/documentation/supann/courant/recommandations/attributs/supanncodeine){:target="_blank"}, Code INE d'un étudiant | 0000003957B | Valeur non obligatoire. La clé peut ne pas être présente ainsi que les '0' en préfixe.  |




<br/>

## Exemple de LDIF

```
dn: uid=agyepong,ou=people,dc=ldap-dev,dc=avenirs-esr,dc=fr
objectClass: top
objectClass: person
objectClass: organizationalPerson
objectClass: inetOrgPerson
objectClass: supannPerson
objectClass: eduPerson
objectClass: eduMember
uid: agyepong
userPassword: {ssha}Ke8lVwbkuJcEbWdCur8XLG9QwggNciz6UlwH/w==
cn: GIANNI AGYEPONG
givenName: GIANNI
displayName: AGYEPONG GIANNI
supannEtuId: 10007474
mail: agyepong@etab.fr
eduPersonPrimaryAffiliation: student
sn: AGYEPONG
supannEtablissement: {UAI}0123456G
supannCivilite: M.
supannOIDCDateDeNaissance: 2003-09-23
supannEntiteAffectation: 909
supannEntiteAffectationPrincipale: 909
eduPersonPrincipalName: agyepong@etab.fr
supannEtuSecteurDisciplinaire: {SISE}0067
supannEtuDiplome: {UAI:0123456G}T3TCO-841
supannEtuTypeDiplome: {SISE}DR
supannEtuEtape: {UAI:0123456G}T3PAD3-100
supannAffectation: L2 STAPS ES parcours LAS
supannEtuCursusAnnee: {SUPANN}L3
supannNomDeNaissance: AGYEPONG
eduPersonAffiliation: student
supannCodeINE: 0000000001B
```

## Exemple de JSON


```json
    {
      "externalId": "agyepong",
      "eppn": "agyepong@avenirs-esr.fr",
      "civility": "M.",
      "firstName": "GIANNI",
      "lastName": "AGYEPONG",
      "displayName": "AGYEPONG GIANNI",
      "audience": "student",
      "INE": "0000000001B",
      "mail": "agyepong@etab.fr",
      "structure": "{UAI}0123456G",
      "cursusYear": "{SUPANN}X2",
      "degree": "{UAI:0123456G}T3TCO-841",
      "degreeType": "{SISE}DR",
      "affectations": [
        "909"
      ],
      "affectationPrincipale": "909",
      "step": "{UAI:0123456G}T3PAD3-100",
      "birthDate": "2003-09-23"
    }
```