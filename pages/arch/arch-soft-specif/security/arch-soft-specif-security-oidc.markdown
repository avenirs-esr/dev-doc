---
layout: page
title: Dossier d'Architecture logicielle - module Security - Intégration OIDC
permalink: /arch-soft-specif-security-oidc/
up: ../../arch-soft-specif-index/
page_content_classes: table-container
---
[Retour](arch-soft-specif-security.markdown)<br/>
<br/>
**Projet :** Avenirs-ESR / ePortfolio. <br/>
**Objet :** Notes relatives à l'intégration OIDC<br/>
<br/>
**Révision :** 1.0.0<br/>
**Date :** 20/12/2024<br/>
**Auteur :** A. Deman<br/>
**Commentaire :** Version initiale<br/>
<br/>

## Flows d'authentification
Actuellement deux types de flows d'authentification sont utilisés :
- [Authorization Code Flow](https://datatracker.ietf.org/doc/html/rfc6749#section-4.1) : redirection du client vers la mire CAS puis redirect vers callback du backend, récupération d'un code, redirection vers le client avec génération de l'access token.<br/>Plus sécurisé, les login/mots de passe ne transitent pas par le backend.
- [Resource Owner Password Credentials (ROPOC) Flow](https://datatracker.ietf.org/doc/html/rfc6749#section-1.3.3). Moins sécurisé, utilisé uniqument dans cadre des tests unitaires pour récupérer un access token sans interraction. <br/>
 Les login/mot de passe transitent par le backend, sans être stockés.

## A propos des claims
 - [Ce warning dans la doc CAS](https://apereo.github.io/cas/development/authentication/OIDC-Authentication-Claims-Release.html) concerne les claims dans l'id token lorsque seul l'id token est demandé, sans access token (response type id_token).
 - Dans notre cas le response type est "code" (Authorization Code Flow), on n'est donc pas dans le cadre strict de ce warning.
 - Le paramètre **cas.authn.oidc.id-token.include-id-token-claims=false** ne change rien pour nous mais, par précaution, il est positionné malgré tout dans cas.properties.

 Malgré cela, c'est une bonne pratique de n'**utiliser l'access token uniquement comme jeton d'authentification et non comme conteneur d'identité.**<br/>
 L'access token doit donc être opaque et ne pas contenir d'information utilisateur. Ces informations sont ensuite récupérées via le end point userinfo ou profile. 

## JWT et claims
L'idée initiale était de continuer à utiliser le format JWT, avec un minimum de claims en limitant le scope à **openid** afin de pouvoir continuer à effectuer des vérifications de validité: signature, expiration, etc.<br/>Cependant cette approche ne fonctionne pas car le scope utilisé pour obtenir l'access token conditionne également le détail des informations retournées par le endpoint profile.

## Access token opaque
Le choix qui est fait est de ne pas utiliser le format JWT pour l'access token - propriété "jwtAccessToken": false dans la définition du service cas.<br/>Avec ce paramétrage, l'access token est opaque, c'est une chaine sans signification particulière et indépendante du scope utilisé (e.g.: openid profile email). Les informations utilisateur peuvent ensuite être récupérées correctement via le end point profile.<br/> 
Les vérifications de sécurité propre au JWT sont conservées mais désactivées via une propriété dans application.properties.<br/><br/>

**La validation de l'access token lors des utilisations ultérieures est réalisée par un appel au endpoint introspect de l'OIDC provider.**

