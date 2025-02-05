---
layout: page
title: Tests de charge
permalink: /load-tests/
up: ../arch/
page_content_classes: table-container
---


# Tests de charge

## Resultats

### 50 utilisateurs concurents
- **Base de données :**  500 utilisateurs et 4836 assignations de rôles.
- [**Rapport locust**](.reports/m1.0/srv-dev-avenir/report-50-5-4.html)
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
