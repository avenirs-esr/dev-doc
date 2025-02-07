---
layout: page
title: Mise en place de tests de charge
permalink: /load-tests/
up: ../arch/
page_content_classes: table-container
---

<br/>
**Projet :** Avenirs-ESR / ePortfolio. <br/>
**Objet :** Méthodologie de tests de charge.<br/>
<br/>
**Révision :** 1.0.0<br/>
**Date :** 03/02/2025<br/>
**Auteur :** A. Deman<br/>
**Commentaire :** Version initiale<br/>
<br/>


## Objectifs
- Mettre en place une méthodologie pour commencer à obtenir des métriques et avoir des repères concernant les temps de réponse cibles pour les différentes API du projet.<br/>
Ces premiers tests concernent un module spécifique, *avenirs-portfolio-security,* chargé de l'intégration OIDC et du contrôle d'accès, de type [RBAC.](https://avenirs-esr.github.io/dev-doc/arch-soft-specif-security-rbac/#concepts){:target="_blank"}<br/>
Cependant, l'objectif est de mettre en place une stratégie transposable pour les autres modules.
- Fournir une base pour quantifier les optimisations ultérieures.
- Pour ce module spécifique, [déterminer la bonne stratégie d’intégration](../arch-soft-specif-security-rbac-integration-experimentations#rbac---intégration-du-contrôle-daccès){:target="_blank"} : directement au niveau de l’API Manager ou au niveau des contrôleurs des différents modules.


## Méthode utilisée

- Génération de fixtures :<br/>
Génération de fixtures de tailles croissantes à l'aide de [Faker.](https://faker.readthedocs.io/en/master/index.html){:target="_blank"}<br/>Le script utilise des valeurs de base et un coefficent multiplicateur pour faire varier la taille des jeux de test : 100 utilisateurs/1000 ressources.<br/>
Chaque utilisateur a entre 1 et 100 rôles assignés.

- Chargement des fixtures dans Postgresql et OpenLDAP (cf [ERD](https://avenirs-esr.github.io/dev-doc/arch-soft-specif-security-rbac-mcd/#rbac---mod%C3%A8le-de-donn%C3%A9es){:target="_blank"})

- Pour chaque jeu de test, exécution de 3 tests de charge en faisant varier le nombre d'accès concurents de 50, 100 et 150 pendant 4 minutes chacun.<br/><br/>
**Séquence de test :**

   1. Sélectionner aléatoirement un utilisateur.
   2. Obtenir un access token pour cet utlisateur, via une requête POST sur le end point /oidc/login.
   3. Effectuer une requête GET sur le end point /roles pour lister les rôles de l'utilisateur.
   4. Selection aléatoire entre 5 et 20 ressources.
   5. Selection aléatoire d'une action.
   6. Pour chaque ressource, l'autorisation de réaliser l'action sur la ressource par l'utlisateur est effectuée via une requête GET sur le end point /access-control/authorize.


## Environnement de test
### Caractéristiques
- Serveur : avenir-srv-dev, machine virtuelle (QEMU/KVM).
- Processeur 4 coeurs, QEMU Virtual CPU version 2.5+.
- Lien  réseau : 1 Gb/s (non partagé pour l'instant, une seule VM sur l'hyperviseur).
- Mémoire : 8 GiB.
- Disque dur : QEMU HARDDISK, Capacité : 130 GB. Intel SSD D3-S4510 Series 2.5" SATA 6.0Gb/s montés en RAID 10.

### Limites et remarques
- L'ensemble des services dockerisés sont exécutés sur un seul serveur.
- Les tests sont exécutés depuis un portable.
- Le lien avec le serveur passe par un VPN.
- Les logs des différents services impliqués sont réduits au maximum.
- Pas d'utilisation de cache.
- Pas d'annalyse ni d'optimisation réalisée au niveau du requêtage et de la base de données.
- Les limites max pour la taille des jeux de données et le nombre d'accès concurents ont été déterminées par les limites matérielles de l'environnement de test et pourront être augmentées dans un autre environnement.

### Améliorations pour obtenir des valeurs plus significatives
- Déployer les services sur une infra.
- Connexion directe, sans VPN.
- Augmenter le volume des données, par exemple en découpant je jeu de test pour le charger.
- Exécuter le test sur plusieurs clients et avec plus d'accès concurrents en utilisant le mode distribué de locust.
- Eventuellement, s'affranchir du reverse proxy.
- Peut-être retravailler les tests pour supprimer la dimension aléatoire, de façon à pouvoir les rejouer à l'identique.


## Resultats

Les limites de l'environnement utilisé rendent les résulats peu significatifs, c'est plus la méthode qui est importante à ce stade. 

Cependant, on peut voir que pour les tests avec 50 accès concurents et une base peu chargée, les temps de réponse sont plutôts bons en regard des conditions de test. <br/>
L'objectif serait de rester proche de ces temps avec une base plus chargée et plus d'accès concurrents. 

Sans surprise, les temps de réponse augmentent avec le nombre d'utilisateurs et le chargement de la base, mais de façon relativement linéaire, ce qui est plutôt rassurant.

### Jeu de test : 100 utilisateurs et 4836 assignations de rôles

- 50 utilisateurs concurents [(voir le rapport  complet locust)](/dev-doc/static-pages/load-tests/reports/m1.0/srv-dev-avenir/report-50-5-4.html){:target="_blank"}
<table border="1">
  <thead>
    <tr>
      <th>Type</th>
      <th>Name</th>
      <th># Requests</th>
      <th># Fails</th>
      <th>Average (ms)</th>
      <th>Min (ms)</th>
      <th>Max (ms)</th>
      <th>Average size (bytes)</th>
      <th>RPS</th>
      <th>Failures/s</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>GET</td>
      <td>/access-control/authorize</td>
      <td>60323</td>
      <td>0</td>
      <td>29.27</td>
      <td>20</td>
      <td>258</td>
      <td>148.25</td>
      <td>251.74</td>
      <td>0</td>
    </tr>
    <tr>
      <td>POST</td>
      <td>/avenirs-portfolio-security/oidc/login</td>
      <td>4826</td>
      <td>0</td>
      <td>28.83</td>
      <td>21</td>
      <td>322</td>
      <td>41</td>
      <td>20.14</td>
      <td>0</td>
    </tr>
    <tr>
      <td>GET</td>
      <td>/avenirs-portfolio-security/roles</td>
      <td>4826</td>
      <td>0</td>
      <td>57.79</td>
      <td>19</td>
      <td>369</td>
      <td>65327.46</td>
      <td>20.14</td>
      <td>0</td>
    </tr>
    <tr>
      <td></td>
      <td>Aggregated</td>
      <td>69975</td>
      <td>0</td>
      <td>31.21</td>
      <td>19</td>
      <td>369</td>
      <td>4636.1</td>
      <td>292.02</td>
      <td>0</td>
    </tr>
  </tbody>
</table>

<br/>

- 100 utilisateurs concurents [(voir le rapport  complet locust)](/dev-doc/static-pages/load-tests/reports/m1.0/srv-dev-avenir/report-100-10-4.html){:target="_blank"}

<table border="1">
  <thead>
    <tr>
      <th>Type</th>
      <th>Name</th>
      <th># Requests</th>
      <th># Fails</th>
      <th>Average (ms)</th>
      <th>Min (ms)</th>
      <th>Max (ms)</th>
      <th>Average size (bytes)</th>
      <th>RPS</th>
      <th>Failures/s</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>GET</td>
      <td>/access-control/authorize</td>
      <td>104193</td>
      <td>0</td>
      <td>56.95</td>
      <td>21</td>
      <td>317</td>
      <td>148.25</td>
      <td>434.45</td>
      <td>0</td>
    </tr>
    <tr>
      <td>POST</td>
      <td>/avenirs-portfolio-security/oidc/login</td>
      <td>8287</td>
      <td>0</td>
      <td>51.96</td>
      <td>22</td>
      <td>344</td>
      <td>41</td>
      <td>34.55</td>
      <td>0</td>
    </tr>
    <tr>
      <td>GET</td>
      <td>/avenirs-portfolio-security/roles</td>
      <td>8282</td>
      <td>1</td>
      <td>86.31</td>
      <td>19</td>
      <td>323</td>
      <td>64988.65</td>
      <td>34.53</td>
      <td>0</td>
    </tr>
    <tr>
      <td></td>
      <td>Aggregated</td>
      <td>120762</td>
      <td>1</td>
      <td>58.62</td>
      <td>19</td>
      <td>344</td>
      <td>4587.72</td>
      <td>503.54</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<br/>

- 150 utilisateurs concurents [(voir le rapport  complet locust)](/dev-doc/static-pages/load-tests/reports/m1.0/srv-dev-avenir/report-150-15-4.html){:target="_blank"}
<table>
  <thead>
    <tr>
      <th>Type</th>
      <th>Name</th>
      <th># Requests</th>
      <th># Fails</th>
      <th>Average (ms)</th>
      <th>Min (ms)</th>
      <th>Max (ms)</th>
      <th>Average size (bytes)</th>
      <th>RPS</th>
      <th>Failures/s</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>GET</td>
      <td>/access-control/authorize</td>
      <td>107871</td>
      <td>0</td>
      <td>147.95</td>
      <td>12</td>
      <td>606</td>
      <td>148.27</td>
      <td>449.67</td>
      <td>0</td>
    </tr>
    <tr>
      <td>POST</td>
      <td>/avenirs-portfolio-security/oidc/login</td>
      <td>8677</td>
      <td>1</td>
      <td>72.92</td>
      <td>22</td>
      <td>491</td>
      <td>41.03</td>
      <td>36.17</td>
      <td>0</td>
    </tr>
    <tr>
      <td>GET</td>
      <td>/avenirs-portfolio-security/roles</td>
      <td>8671</td>
      <td>0</td>
      <td>180.65</td>
      <td>23</td>
      <td>569</td>
      <td>64895.38</td>
      <td>36.15</td>
      <td>0</td>
    </tr>
    <tr>
      <td></td>
      <td>Aggregated</td>
      <td>125219</td>
      <td>1</td>
      <td>145.02</td>
      <td>12</td>
      <td>606</td>
      <td>4624.36</td>
      <td>521.99</td>
      <td>0</td>
    </tr>
  </tbody>
</table>

<br/><br/>


### Jeu de test : 100 utilisateurs 23568 assignations de rôles

- 50 utilisateurs concurents [(voir le rapport  complet locust)](/dev-doc/static-pages/load-tests/reports/m5.0/srv-dev-avenir/report-50-5-4.html){:target="_blank"}
<table border="1">
  <thead>
    <tr>
      <th>Type</th>
      <th>Name</th>
      <th># Requests</th>
      <th># Fails</th>
      <th>Average (ms)</th>
      <th>Min (ms)</th>
      <th>Max (ms)</th>
      <th>Average size (bytes)</th>
      <th>RPS</th>
      <th>Failures/s</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>GET</td>
      <td>/access-control/authorize</td>
      <td>51121</td>
      <td>0</td>
      <td>62.69</td>
      <td>24</td>
      <td>1566</td>
      <td>149.22</td>
      <td>213.19</td>
      <td>0</td>
    </tr>
    <tr>
      <td>POST</td>
      <td>/avenirs-portfolio-security/oidc/login</td>
      <td>4080</td>
      <td>0</td>
      <td>30.75</td>
      <td>21</td>
      <td>270</td>
      <td>41</td>
      <td>17.01</td>
      <td>0</td>
    </tr>
    <tr>
      <td>GET</td>
      <td>/avenirs-portfolio-security/roles</td>
      <td>4079</td>
      <td>0</td>
      <td>79.32</td>
      <td>20</td>
      <td>1351</td>
      <td>61528.93</td>
      <td>17.01</td>
      <td>0</td>
    </tr>
    <tr>
      <td></td>
      <td>Aggregated</td>
      <td>59280</td>
      <td>0</td>
      <td>61.64</td>
      <td>20</td>
      <td>1566</td>
      <td>4365.25</td>
      <td>247.21</td>
      <td>0</td>
    </tr>
  </tbody>
</table>



<br/>
- 100 utilisateurs concurents [(voir le rapport  complet locust)](/dev-doc/static-pages/load-tests/reports/m5.0/srv-dev-avenir/report-100-10-4.html){:target="_blank"}
<table border="1">
  <thead>
    <tr>
      <th>Type</th>
      <th>Name</th>
      <th># Requests</th>
      <th># Fails</th>
      <th>Average (ms)</th>
      <th>Min (ms)</th>
      <th>Max (ms)</th>
      <th>Average size (bytes)</th>
      <th>RPS</th>
      <th>Failures/s</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>GET</td>
      <td>/access-control/authorize</td>
      <td>67923</td>
      <td>0</td>
      <td>170.19</td>
      <td>25</td>
      <td>525</td>
      <td>149.22</td>
      <td>283.17</td>
      <td>0</td>
    </tr>
    <tr>
      <td>POST</td>
      <td>/avenirs-portfolio-security/oidc/login</td>
      <td>5456</td>
      <td>0</td>
      <td>34.46</td>
      <td>22</td>
      <td>161</td>
      <td>41</td>
      <td>22.75</td>
      <td>0</td>
    </tr>
    <tr>
      <td>GET</td>
      <td>/avenirs-portfolio-security/roles</td>
      <td>5450</td>
      <td>0</td>
      <td>186.44</td>
      <td>23</td>
      <td>466</td>
      <td>61820.13</td>
      <td>22.72</td>
      <td>0</td>
    </tr>
    <tr>
      <td></td>
      <td>Aggregated</td>
      <td>78829</td>
      <td>0</td>
      <td>161.92</td>
      <td>22</td>
      <td>525</td>
      <td>4405.47</td>
      <td>328.63</td>
      <td>0</td>
    </tr>
  </tbody>
</table>


<br/>
- 150 utilisateurs concurents [(voir le rapport  complet locust)](/dev-doc/static-pages/load-tests/reports/m5.0/srv-dev-avenir/report-150-15-4.html){:target="_blank"}

<table border="1">
  <thead>
    <tr>
      <th>Type</th>
      <th>Name</th>
      <th># Requests</th>
      <th># Fails</th>
      <th>Average (ms)</th>
      <th>Min (ms)</th>
      <th>Max (ms)</th>
      <th>Average size (bytes)</th>
      <th>RPS</th>
      <th>Failures/s</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>GET</td>
      <td>/access-control/authorize</td>
      <td>67789</td>
      <td>0</td>
      <td>330.08</td>
      <td>25</td>
      <td>1060</td>
      <td>149.19</td>
      <td>282.59</td>
      <td>0</td>
    </tr>
    <tr>
      <td>POST</td>
      <td>/avenirs-portfolio-security/oidc/login</td>
      <td>5516</td>
      <td>0</td>
      <td>36.81</td>
      <td>22</td>
      <td>265</td>
      <td>41</td>
      <td>22.99</td>
      <td>0</td>
    </tr>
    <tr>
      <td>GET</td>
      <td>/avenirs-portfolio-security/roles</td>
      <td>5511</td>
      <td>0</td>
      <td>345.95</td>
      <td>24</td>
      <td>947</td>
      <td>62256.09</td>
      <td>22.97</td>
      <td>0</td>
    </tr>
    <tr>
      <td></td>
      <td>Aggregated</td>
      <td>78816</td>
      <td>0</td>
      <td>310.66</td>
      <td>22</td>
      <td>1060</td>
      <td>4484.28</td>
      <td>328.56</td>
      <td>0</td>
    </tr>
  </tbody>
</table>

<br/><br/>

### Jeu de test :  2000 utilisateurs 98650 assignations de rôles

- 50 utilisateurs concurents [(voir le rapport  complet locust)](/dev-doc/static-pages/load-tests/reports/m20.0/srv-dev-avenir/report-50-5-4.html){:target="_blank"}

<table border="1">
    <thead>
        <tr>
            <th>Type</th>
            <th>Name</th>
            <th># Requests</th>
            <th># Fails</th>
            <th>Average (ms)</th>
            <th>Min (ms)</th>
            <th>Max (ms)</th>
            <th>Average size (bytes)</th>
            <th>RPS</th>
            <th>Failures/s</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>GET</td>
            <td>/access-control/authorize</td>
            <td>21119</td>
            <td>0</td>
            <td>356.66</td>
            <td>101</td>
            <td>857</td>
            <td>149.99</td>
            <td>88.2</td>
            <td>0</td>
        </tr>
        <tr>
            <td>POST</td>
            <td>/avenirs-portfolio-security/oidc/login</td>
            <td>1718</td>
            <td>0</td>
            <td>77.69</td>
            <td>52</td>
            <td>523</td>
            <td>42</td>
            <td>7.17</td>
            <td>0</td>
        </tr>
        <tr>
            <td>GET</td>
            <td>/avenirs-portfolio-security/roles</td>
            <td>1717</td>
            <td>0</td>
            <td>416.74</td>
            <td>83</td>
            <td>1768</td>
            <td>64749.38</td>
            <td>7.17</td>
            <td>0</td>
        </tr>
        <tr>
            <td></td>
            <td>Aggregated</td>
            <td>24554</td>
            <td>0</td>
            <td>341.34</td>
            <td>52</td>
            <td>1768</td>
            <td>4659.71</td>
            <td>102.55</td>
            <td>0</td>
        </tr>
    </tbody>
</table>




<br/>
- 100 utilisateurs concurents [(voir le rapport  complet locust)](/dev-doc/static-pages/load-tests/reports/m20.0/srv-dev-avenir/report-100-10-4.html){:target="_blank"}

<table border="1">
    <thead>
        <tr>
            <th>Type</th>
            <th>Name</th>
            <th># Requests</th>
            <th># Fails</th>
            <th>Average (ms)</th>
            <th>Min (ms)</th>
            <th>Max (ms)</th>
            <th>Average size (bytes)</th>
            <th>RPS</th>
            <th>Failures/s</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>GET</td>
            <td>/access-control/authorize</td>
            <td>22183</td>
            <td>0</td>
            <td>825.97</td>
            <td>76</td>
            <td>2334</td>
            <td>149.92</td>
            <td>92.52</td>
            <td>0</td>
        </tr>
        <tr>
            <td>POST</td>
            <td>/avenirs-portfolio-security/oidc/login</td>
            <td>1822</td>
            <td>0</td>
            <td>76.71</td>
            <td>52</td>
            <td>604</td>
            <td>42</td>
            <td>7.6</td>
            <td>0</td>
        </tr>
        <tr>
            <td>GET</td>
            <td>/avenirs-portfolio-security/roles</td>
            <td>1815</td>
            <td>0</td>
            <td>864.12</td>
            <td>74</td>
            <td>1734</td>
            <td>64428.29</td>
            <td>7.57</td>
            <td>0</td>
        </tr>
        <tr>
            <td></td>
            <td>Aggregated</td>
            <td>25820</td>
            <td>0</td>
            <td>775.78</td>
            <td>52</td>
            <td>2334</td>
            <td>4660.71</td>
            <td>107.69</td>
            <td>0</td>
        </tr>
    </tbody>
</table>


<br/>
- 150 utilisateurs concurents [(voir le rapport  complet locust)](/dev-doc/static-pages/load-tests/reports/m20.0/srv-dev-avenir/report-150-15-4.html){:target="_blank"}

<table border="1">
    <thead>
        <tr>
            <th>Type</th>
            <th>Name</th>
            <th># Requests</th>
            <th># Fails</th>
            <th>Average (ms)</th>
            <th>Min (ms)</th>
            <th>Max (ms)</th>
            <th>Average size (bytes)</th>
            <th>RPS</th>
            <th>Failures/s</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>GET</td>
            <td>/access-control/authorize</td>
            <td>21981</td>
            <td>0</td>
            <td>1323.97</td>
            <td>67</td>
            <td>3777</td>
            <td>149.9</td>
            <td>91.73</td>
            <td>0</td>
        </tr>
        <tr>
            <td>POST</td>
            <td>/avenirs-portfolio-security/oidc/login</td>
            <td>1859</td>
            <td>0</td>
            <td>82.31</td>
            <td>50</td>
            <td>616</td>
            <td>42</td>
            <td>7.76</td>
            <td>0</td>
        </tr>
        <tr>
            <td>GET</td>
            <td>/avenirs-portfolio-security/roles</td>
            <td>1849</td>
            <td>1</td>
            <td>1337.2</td>
            <td>75</td>
            <td>2803</td>
            <td>63392.61</td>
            <td>7.72</td>
            <td>0</td>
        </tr>
        <tr>
            <td></td>
            <td>Aggregated</td>
            <td>25689</td>
            <td>1</td>
            <td>1235.07</td>
            <td>50</td>
            <td>3777</td>
            <td>4694.07</td>
            <td>107.2</td>
            <td>0</td>
        </tr>
    </tbody>
</table>


<br/><br/>

### Jeu de test :  3000 utilisateurs 152315 assignations de rôles
- 50 utilisateurs concurents [(voir le rapport  complet locust)](/dev-doc/static-pages/load-tests/reports/m30.0/srv-dev-avenir/report-50-5-4.html){:target="_blank"}

<table border="1">
  <thead>
    <tr>
      <th>Type</th>
      <th>Name</th>
      <th># Requests</th>
      <th># Fails</th>
      <th>Average (ms)</th>
      <th>Min (ms)</th>
      <th>Max (ms)</th>
      <th>Average size (bytes)</th>
      <th>RPS</th>
      <th>Failures/s</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>GET</td>
      <td>/access-control/authorize</td>
      <td>15214</td>
      <td>0</td>
      <td>550.75</td>
      <td>97</td>
      <td>2850</td>
      <td>150.08</td>
      <td>63.5</td>
      <td>0</td>
    </tr>
    <tr>
      <td>POST</td>
      <td>/avenirs-portfolio-security/oidc/login</td>
      <td>1246</td>
      <td>0</td>
      <td>104.41</td>
      <td>53</td>
      <td>751</td>
      <td>42</td>
      <td>5.2</td>
      <td>0</td>
    </tr>
    <tr>
      <td>GET</td>
      <td>/avenirs-portfolio-security/roles</td>
      <td>1243</td>
      <td>0</td>
      <td>682.18</td>
      <td>196</td>
      <td>4746</td>
      <td>66262.5</td>
      <td>5.19</td>
      <td>0</td>
    </tr>
    <tr>
      <td></td>
      <td>Aggregated</td>
      <td>17703</td>
      <td>0</td>
      <td>528.56</td>
      <td>53</td>
      <td>4746</td>
      <td>4784.49</td>
      <td>73.89</td>
      <td>0</td>
    </tr>
  </tbody>
</table>


<br/>
- 100 utilisateurs concurents [(voir le rapport  complet locust)](/dev-doc/static-pages/load-tests/reports/m30.0/srv-dev-avenir/report-100-10-4.html){:target="_blank"}

<table border="1">
  <thead>
    <tr>
      <th>Type</th>
      <th>Name</th>
      <th># Requests</th>
      <th># Fails</th>
      <th>Average (ms)</th>
      <th>Min (ms)</th>
      <th>Max (ms)</th>
      <th>Average size (bytes)</th>
      <th>RPS</th>
      <th>Failures/s</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>GET</td>
      <td>/access-control/authorize</td>
      <td>15781</td>
      <td>0</td>
      <td>1211.55</td>
      <td>32</td>
      <td>3672</td>
      <td>150.07</td>
      <td>65.82</td>
      <td>0</td>
    </tr>
    <tr>
      <td>POST</td>
      <td>/avenirs-portfolio-security/oidc/login</td>
      <td>1334</td>
      <td>0</td>
      <td>114.75</td>
      <td>49</td>
      <td>2053</td>
      <td>42</td>
      <td>5.56</td>
      <td>0</td>
    </tr>
    <tr>
      <td>GET</td>
      <td>/avenirs-portfolio-security/roles</td>
      <td>1331</td>
      <td>0</td>
      <td>1273.82</td>
      <td>89</td>
      <td>6155</td>
      <td>68725.64</td>
      <td>5.55</td>
      <td>0</td>
    </tr>
    <tr>
      <td></td>
      <td>Aggregated</td>
      <td>18446</td>
      <td>0</td>
      <td>1136.72</td>
      <td>32</td>
      <td>6155</td>
      <td>5090.43</td>
      <td>76.93</td>
      <td>0</td>
    </tr>
  </tbody>
</table>


<br/>
- 150 utilisateurs concurents [(voir le rapport  complet locust)](/dev-doc/static-pages/load-tests/reports/m30.0/srv-dev-avenir/report-150-15-4.html){:target="_blank"}

<table border="1">
  <thead>
    <tr>
      <th>Type</th>
      <th>Name</th>
      <th># Requests</th>
      <th># Fails</th>
      <th>Average (ms)</th>
      <th>Min (ms)</th>
      <th>Max (ms)</th>
      <th>Average size (bytes)</th>
      <th>RPS</th>
      <th>Failures/s</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>GET</td>
      <td>/access-control/authorize</td>
      <td>15863</td>
      <td>0</td>
      <td>1892.97</td>
      <td>45</td>
      <td>5302</td>
      <td>150</td>
      <td>66.15</td>
      <td>0</td>
    </tr>
    <tr>
      <td>POST</td>
      <td>/avenirs-portfolio-security/oidc/login</td>
      <td>1373</td>
      <td>3</td>
      <td>89.86</td>
      <td>48</td>
      <td>652</td>
      <td>41.91</td>
      <td>5.73</td>
      <td>0.01</td>
    </tr>
    <tr>
      <td>GET</td>
      <td>/avenirs-portfolio-security/roles</td>
      <td>1367</td>
      <td>1</td>
      <td>1864.85</td>
      <td>79</td>
      <td>3832</td>
      <td>68449.28</td>
      <td>5.7</td>
      <td>0</td>
    </tr>
    <tr>
      <td></td>
      <td>Aggregated</td>
      <td>18603</td>
      <td>4</td>
      <td>1757.82</td>
      <td>45</td>
      <td>5302</td>
      <td>5160.85</td>
      <td>77.58</td>
      <td>0.02</td>
    </tr>
  </tbody>
</table>


<br/><br/>


<!--
### Jeu de test :  utilisateurs  assignations de rôles
- 50 utilisateurs concurents [(voir le rapport  complet locust)](/dev-doc/static-pages/load-tests/reports/m10.0/srv-dev-avenir/report-50-5-4.html){:target="_blank"}



<br/>
- 100 utilisateurs concurents [(voir le rapport  complet locust)](/dev-doc/static-pages/load-tests/reports/m10.0/srv-dev-avenir/report-100-10-4.html){:target="_blank"}



<br/>
- 150 utilisateurs concurents [(voir le rapport  complet locust)](/dev-doc/static-pages/load-tests/reports/m10.0/srv-dev-avenir/report-150-15-4.html){:target="_blank"}

<br/><br/>

-->