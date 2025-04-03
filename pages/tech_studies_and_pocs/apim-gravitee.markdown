---
layout: page
title: Gravitee
permalink: /apim-gravitee/
---


## Resources

- [gravitee.io](https://documentation.gravitee.io/apim/overview/readme){:target="_blank"}



## Installation notes

The installation method used is [Custom Install with Docker Compose](https://documentation.gravitee.io/apim/getting-started/install-guides/install-on-docker/custom-install-with-docker-compose){:target="_blank"}.
In this installation mode a directory structure has to be created. It is not mentionned in the doc but under debian like distros, the owner/groups of some folders have to be adjusted (this could probably done via the docker file too).

<pre>
/gravitee
 ├── docker-compose-apim.yaml
 ├── apim-gateway     
 │    ├── logs                   graviteeio (user defined in the image with the uid 1000)
 │    └── plugins
 ├── apim-management-api
 │    ├── logs                   systemd-resolve systemd-journal
 │    └── plugins
 ├── apim-management-ui
 │    └── logs
 ├── apim-portal-ui
 │    └── logs                   systemd-resolve systemd-journal
 ├── elasticsearch
 │    └── data
 └── mongodb                     
     └── data                    systemd-coredump systemd-coredump

</pre>

## OIDC integration with CAS

* [Configure CAS as an OIDC Provider & define a service.](../apim-cas-oidc-provider){:target="_blank"}


* Create a resource of type OAuth2: 
 {%  include img.html  
        src='assets/images/gravitee/gravitee_resource_oidc.png'
        alt='Gravitee OIDC resource'
 %}

### Test
* Fetch the access token. For instance this can be done via an url of the form:<br/>
https://casdev.univ-tln.fr/cas/oidc/oidcAuthorize?client_id=GraviteeClientId&redirect_uri=https://dev-backend.univ-tln.fr/cas-auth-callback&response_type=code&scope=openid%20profile%20email

* Test with curl:
curl -H "Authorization: Bearer AT-6-CBX0teJBhCM5VECay3JBgj78UCuPbttL" http://localhost:8082/cas-oidc


### Troubleshooting

Gravitee does not seem to send the Content-Type in the Header of the introspect query. <br/>
As a consequence the query is rejected with an HTTP Error 415 (CAS server) and with an HTTP Error 401 (Gravitee). The Apache's module fortensic can be use to log the headers.<br/>
The Transform headers policy of Gravitee does not seem to be triggered for the introspection queries. A solution could be to add it via Apache (module Headers):
<pre>
    <VirtualHost *:443>
      (...)
    	<Location /cas/oidc/introspect>
    		   RequestHeader set Content-type "application/json"
    	</Location>
      (...)
    </VirtualHost>
</pre>


