---
layout: page
title: Tests de charge
permalink: /load-tests/
up: ../arch/
page_content_classes: table-container
---


# Tests de charge

## Resultats

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


# Jeu de test : 100 utilisateurs 23568 assignations de rôles

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
