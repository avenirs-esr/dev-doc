---
layout: page
title: Apache APISSIX
permalink: /apim-apisix/
---


## Resources

- [APISIX Homepage](https://apisix.apache.org/){:target="_blank"}
- [APISix Documentation](https://apisix.apache.org/docs/apisix/getting-started/README/){:target="_blank"}
- [Openid-Connect plugin](https://apisix.apache.org/docs/apisix/plugins/openid-connect/){:target="_blank"}
- [Use OpenID Connect to Protect Your APIs](https://docs.api7.ai/cloud/guides/traffic-management/authentication/openid-connect){:target="_blank"}

## Installation notes
* Installation done via the [docker method](https://apisix.apache.org/docs/apisix/installation-guide/){:target="_blank"}.

<pre>
     git clone https://github.com/apache/apisix-docker.git
     cd apisix-docker/example
     docker-compose -p docker-apisix up -d   
</pre>

The configuration file is **apisix-docker/example/apisix_conf/config.yaml.** It contains an admin key which will be used to interact with APISIX's API.



Once the docker containers are launched the UI is accessible at this address: http://localhost:9000<br/>
The routes can be configured via the UI or directly via APISIX API.

## OIDC Integration with CAS

 
* Create a route (API Key found in apisix_conf/config.yaml)
<pre>
        curl -H "X-API-KEY: edd1c9f034335f136f87ad84b625c8f1" -i "http://127.0.0.1:9180/apisix/admin/routes" -X PUT -d '
        {
          "id": "test-oidc",
          "uri": "/cas-oidc2",
          "plugins": {
              "proxy-rewrite": {
                "uri": "/health"
              }
            },
          "upstream": {
            "type": "roundrobin",
            "scheme": "http",
            "nodes": {
              "10.2.64.17:3000": 1

            }
          }
        }'
</pre>

* Enable and parameter the oidc plugin

<pre>
        curl -H "X-API-KEY: edd1c9f034335f136f87ad84b625c8f1"  "http://127.0.0.1:9180/apisix/admin/routes/test-oidc" -X PATCH -d '
        {
            "plugins": {
                "openid-connect": {
                    "_meta": {
                        "disable": false
                    },
                    "bearer_only": true,
                    "client_id": "GraviteeClientId",
                    "client_secret": "ErT322hVLHzIi9Z5tbu58yzUvzVqlsh3T0tmKRV41bu004wqY664TM=",
                    "discovery": "https://casdev.univ-tln.fr/cas/oidc/.well-known",
                    "introspection_endpoint": "https://casdev.univ-tln.fr/cas/oidc/introspect",
                    "redirect_uri": "https://dev-backend.univ-tln.fr/cas-auth-callback",
                    "scope": "openid profile email",
                    "set_access_token_header": true,
                    "token_endpoint_auth_method": "GET"
                }
            }
        }'
</pre>

### Test

* Fetch the access token. For instance this can be done via an url of the form:<br/>
https://casdev.univ-tln.fr/cas/oidc/oidcAuthorize?client_id=GraviteeClientId&redirect_uri=https://dev-backend.univ-tln.fr/cas-auth-callback&response_type=code&scope=openid%20profile%20email

* Test with curl:
 curl -H "Authorization: Bearer AT-8-p37ndyGEPPqeygnYVyIDL9GaCSQt7qx6" "http://127.0.0.1:9080/cas-oidc"


