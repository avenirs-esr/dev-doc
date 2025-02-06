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

<br/><br/>

# Jeu de test : 1000 utilisateurs 622799 assignations de rôles

- 50 utilisateurs concurents [(voir le rapport  complet locust)](/dev-doc/static-pages/load-tests/reports/m10.0/srv-dev-avenir/report-50-5-4.html){:target="_blank"}



<br/>
- 100 utilisateurs concurents [(voir le rapport  complet locust)](/dev-doc/static-pages/load-tests/reports/m10.0/srv-dev-avenir/report-100-10-4.html){:target="_blank"}



<br/>
- 150 utilisateurs concurents [(voir le rapport  complet locust)](/dev-doc/static-pages/load-tests/reports/m10.0/srv-dev-avenir/report-150-15-4.html){:target="_blank"}

<br/><br/>

# Jeu de test : 5000 utilisateurs  assignations de rôles

- 50 utilisateurs concurents [(voir le rapport  complet locust)](/dev-doc/static-pages/load-tests/reports/m50.0/srv-dev-avenir/report-50-5-4.html){:target="_blank"}

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
      <td>44907</td>
      <td>0</td>
      <td>86.55</td>
      <td>47</td>
      <td>663</td>
      <td>0</td>
      <td>187.42</td>
      <td>0</td>
    </tr>
    <tr>
      <td>POST</td>
      <td>/avenirs-portfolio-security/oidc/login</td>
      <td>3618</td>
      <td>0</td>
      <td>88.71</td>
      <td>50</td>
      <td>683</td>
      <td>41</td>
      <td>15.1</td>
      <td>0</td>
    </tr>
    <tr>
      <td>GET</td>
      <td>/avenirs-portfolio-security/roles</td>
      <td>3618</td>
      <td>0</td>
      <td>85.95</td>
      <td>47</td>
      <td>653</td>
      <td>2</td>
      <td>15.1</td>
      <td>0</td>
    </tr>
    <tr>
      <td></td>
      <td>Aggregated</td>
      <td>52143</td>
      <td>0</td>
      <td>86.66</td>
      <td>47</td>
      <td>683</td>
      <td>2.98</td>
      <td>217.62</td>
      <td>0</td>
    </tr>
  </tbody>
</table>


<br/>
- 100 utilisateurs concurents [(voir le rapport  complet locust)](/dev-doc/static-pages/load-tests/reports/m50.0/srv-dev-avenir/report-100-10-4.html){:target="_blank"}


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
      <td>89376</td>
      <td>0</td>
      <td>89.32</td>
      <td>45</td>
      <td>1873</td>
      <td>0</td>
      <td>372.82</td>
      <td>0</td>
    </tr>
    <tr>
      <td>POST</td>
      <td>/avenirs-portfolio-security/oidc/login</td>
      <td>7160</td>
      <td>0</td>
      <td>92.93</td>
      <td>50</td>
      <td>558</td>
      <td>41</td>
      <td>29.87</td>
      <td>0</td>
    </tr>
    <tr>
      <td>GET</td>
      <td>/avenirs-portfolio-security/roles</td>
      <td>7156</td>
      <td>0</td>
      <td>91.67</td>
      <td>49</td>
      <td>402</td>
      <td>2</td>
      <td>29.85</td>
      <td>0</td>
    </tr>
    <tr>
      <td></td>
      <td>Aggregated</td>
      <td>103692</td>
      <td>0</td>
      <td>89.73</td>
      <td>45</td>
      <td>1873</td>
      <td>2.97</td>
      <td>432.54</td>
      <td>0</td>
    </tr>
  </tbody>
</table>

<br/>
- 150 utilisateurs concurents [(voir le rapport  complet locust)](/dev-doc/static-pages/load-tests/reports/m50.0/srv-dev-avenir/report-150-15-4.html){:target="_blank"}

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
      <td>130764</td>
      <td>0</td>
      <td>93.5</td>
      <td>48</td>
      <td>1730</td>
      <td>0</td>
      <td>545.41</td>
      <td>0</td>
    </tr>
    <tr>
      <td>POST</td>
      <td>/avenirs-portfolio-security/oidc/login</td>
      <td>10512</td>
      <td>2</td>
      <td>104.31</td>
      <td>40</td>
      <td>640</td>
      <td>41.33</td>
      <td>43.84</td>
      <td>0.01</td>
    </tr>
    <tr>
      <td>GET</td>
      <td>/avenirs-portfolio-security/roles</td>
      <td>10506</td>
      <td>0</td>
      <td>98.92</td>
      <td>50</td>
      <td>839</td>
      <td>2</td>
      <td>43.82</td>
      <td>0</td>
    </tr>
    <tr>
      <td></td>
      <td>Aggregated</td>
      <td>151782</td>
      <td>2</td>
      <td>94.62</td>
      <td>40</td>
      <td>1730</td>
      <td>3</td>
      <td>633.07</td>
      <td>0.01</td>
    </tr>
  </tbody>
</table>


# Jeu de test : 10000 utilisateurs  assignations de rôles

- 50 utilisateurs concurents [(voir le rapport  complet locust)](/dev-doc/static-pages/load-tests/reports/m100.0/srv-dev-avenir/report-50-5-4.html){:target="_blank"}

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
      <td>45745</td>
      <td>0</td>
      <td>84.91</td>
      <td>46</td>
      <td>671</td>
      <td>0</td>
      <td>191.03</td>
      <td>0</td>
    </tr>
    <tr>
      <td>POST</td>
      <td>/avenirs-portfolio-security/oidc/login</td>
      <td>3673</td>
      <td>0</td>
      <td>83.68</td>
      <td>49</td>
      <td>439</td>
      <td>41</td>
      <td>15.34</td>
      <td>0</td>
    </tr>
    <tr>
      <td>GET</td>
      <td>/avenirs-portfolio-security/roles</td>
      <td>3672</td>
      <td>0</td>
      <td>83.31</td>
      <td>48</td>
      <td>538</td>
      <td>2</td>
      <td>15.33</td>
      <td>0</td>
    </tr>
    <tr>
      <td></td>
      <td>Aggregated</td>
      <td>53090</td>
      <td>0</td>
      <td>84.72</td>
      <td>46</td>
      <td>671</td>
      <td>2.97</td>
      <td>221.71</td>
      <td>0</td>
    </tr>
  </tbody>
</table>


<br/>
- 100 utilisateurs concurents [(voir le rapport  complet locust)](/dev-doc/static-pages/load-tests/reports/m100.0/srv-dev-avenir/report-100-10-4.html){:target="_blank"}

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
      <td>92486</td>
      <td>0</td>
      <td>81.29</td>
      <td>46</td>
      <td>1132</td>
      <td>0</td>
      <td>386</td>
      <td>0</td>
    </tr>
    <tr>
      <td>POST</td>
      <td>/avenirs-portfolio-security/oidc/login</td>
      <td>7430</td>
      <td>0</td>
      <td>84.33</td>
      <td>52</td>
      <td>416</td>
      <td>41</td>
      <td>31.01</td>
      <td>0</td>
    </tr>
    <tr>
      <td>GET</td>
      <td>/avenirs-portfolio-security/roles</td>
      <td>7428</td>
      <td>0</td>
      <td>82.11</td>
      <td>49</td>
      <td>470</td>
      <td>2</td>
      <td>31</td>
      <td>0</td>
    </tr>
    <tr>
      <td></td>
      <td>Aggregated</td>
      <td>107344</td>
      <td>0</td>
      <td>81.56</td>
      <td>46</td>
      <td>1132</td>
      <td>2.98</td>
      <td>448.02</td>
      <td>0</td>
    </tr>
  </tbody>
</table>



<br/>
- 150 utilisateurs concurents [(voir le rapport  complet locust)](/dev-doc/static-pages/load-tests/reports/m100.0/srv-dev-avenir/report-150-15-4.html){:target="_blank"}

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
      <td>132918</td>
      <td>0</td>
      <td>89.75</td>
      <td>38</td>
      <td>2017</td>
      <td>0</td>
      <td>554.61</td>
      <td>0</td>
    </tr>
    <tr>
      <td>POST</td>
      <td>/avenirs-portfolio-security/oidc/login</td>
      <td>10668</td>
      <td>4</td>
      <td>100.68</td>
      <td>52</td>
      <td>1163</td>
      <td>40.98</td>
      <td>44.51</td>
      <td>0.02</td>
    </tr>
    <tr>
      <td>GET</td>
      <td>/avenirs-portfolio-security/roles</td>
      <td>10660</td>
      <td>3</td>
      <td>94.15</td>
      <td>50</td>
      <td>979</td>
      <td>2</td>
      <td>44.48</td>
      <td>0.01</td>
    </tr>
    <tr>
      <td></td>
      <td>Aggregated</td>
      <td>154246</td>
      <td>7</td>
      <td>90.81</td>
      <td>38</td>
      <td>2017</td>
      <td>2.97</td>
      <td>643.6</td>
      <td>0.03</td>
    </tr>
  </tbody>
</table>
