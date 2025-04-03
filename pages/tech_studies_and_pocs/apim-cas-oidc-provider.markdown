---
layout: page
title: CAS - OIDC provider
permalink: /apim-cas-oidc-provider/
---



* Enable CAS to [act as an OpenId Connect Provider.](https://apereo.github.io/cas/7.0.x/authentication/OIDC-Authentication.html){:target="_blank"}<br/><br/>
    * Checks the OIDC Configuration via this end point :<br/>
 https://myserver.univ.fr/cas/oidc/.well-known/openid-configuration
    * The output should have this form :
<pre>
            issuer	"https://casdev.univ-tln.fr/cas/oidc"
            scopes_supported	
                0	"openid"
                1	"profile"
                2	"email"
                3	"phone"
                4	"offline_access"
            (...)

            authorization_endpoint                  "https://casdev.univ-tln.fr/cas/oidc/oidcAuthorize"
            token_endpoint                          "https://casdev.univ-tln.fr/cas/oidc/oidcAccessToken"
            userinfo_endpoint                       "https://casdev.univ-tln.fr/cas/oidc/oidcProfile"
            pushed_authorization_request_endpoint   "https://casdev.univ-tln.fr/cas/oidc/oidcPushAuthorize"
            registration_endpoint                   "https://casdev.univ-tln.fr/cas/oidc/register"
            end_session_endpoint                    "https://casdev.univ-tln.fr/cas/oidc/oidcLogout"
            introspection_endpoint                  "https://casdev.univ-tln.fr/cas/oidc/introspect"
            revocation_endpoint                     "https://casdev.univ-tln.fr/cas/oidc/revoke"
            backchannel_logout_session_supported    true
            frontchannel_logout_session_supported   true    
</pre>

* Define a CAS service<br/>
 N.B.: the **clientId** and **clientSecret** are defined in the service description.<br/>
 The service is associated to the callback URL via the **serviceId.**

<pre>


            {
              "id": 4000,
              "name": "APIM",
              "@class": "org.apereo.cas.services.OidcRegisteredService",
              "bypassApprovalPrompt": true,
              "clientId": "MyClientId",
              "clientSecret": "ErT32*******************************64TM=",
              "supportedGrantTypes": [ "java.util.HashSet", [ "authorization_code" ] ],
              "supportedResponseTypes": [ "java.util.HashSet", [ "code", "token" ] ]
              "serviceId": "^https://dev-backend.univ-tln.fr/cas-auth-callback(/access)?$",
              "scopes": [
                "java.util.HashSet",
                [
                  "openid",
                  "profile",
                  "email",
                ]
              ],
              "attributeReleasePolicy": {
                "@class": "org.apereo.cas.services.ReturnMappedAttributeReleasePolicy",
                  "allowedAttributes": {
            	  "@class": "java.util.TreeMap",
            	  "mail": "email",
            	  "cn": "name",
            	  "sn": "family_name",
            	  "givenName": "given_name",
                  }
              }
            }

</pre>



