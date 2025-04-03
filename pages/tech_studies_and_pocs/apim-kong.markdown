---
layout: page
title: Kong
permalink: /apim-kong/
---


## Resources

- [Kong Homepage](https://konghq.com/){:target="_blank"}
- [GitHub Project](https://github.com/Kong/kong){:target="_blank"}
- [Kong documentation](https://docs.konghq.com/){:target="_blank"}


## Installation notes
* Installation done via the [docker method](https://docs.konghq.com/gateway/3.4.x/get-started/){:target="_blank"}.

<pre>
    curl -Ls https://get.konghq.com/quickstart | bash
</pre>

The above command will install the gateway (with the manager)  and a Postgres database.
<pre>
    f2910e38d492   kong/kong-gateway:3.4.1.0    "/entrypoint.sh kong…"   3 hours ago   Up 3 hours (healthy)   0.0.0.0:8000-8004->8000-8004/tcp,::8000-8004->8000-8004/tcp, 8443-8447/tcp  kong-quickstart-gateway
    6acbad818392   postgres:13                   "docker-entrypoint.s…"   3 hours ago   Up 3 hours            5432/tcp 
</pre>

## Tests
- Key less: straightforward
- API Key: use of the [key Auth Plugin](https://docs.konghq.com/hub/kong-inc/key-auth/){:target="_blank"}. See also the [Authentication Reference page](https://docs.konghq.com/gateway/3.4.x/kong-plugins/authentication/reference/){:target="_blank"}  for the settings.
- OIDC: The plugin [OIDC](https://docs.konghq.com/hub/kong-inc/openid-connect/){:target="_blank"} is only available for th Enterprise Licence.

## On-premise installation
- The Enterprise licence is required to go on production, [a lot of functionnalities](https://docs.konghq.com/gateway/latest/kong-enterprise/) are only available with a subscription (e.g. Plugins, Developper Portal, etc).
- Kong can be installed locally but the [pricing page](https://konghq.com/pricing){:target="_blank"} seems to refer to the SAAS mode.