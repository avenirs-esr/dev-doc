---
layout: page
title: List of API Management tools (not cloud based)
permalink: /apim-list/
page_content_classes: table-container
---



<table>
    <thead>
        <th>Type</th>
        <th> Open source </th> 
        <th>Community / Enterprise edition </th>
        <th>Main features</th> 
        <th>Protocol mediation</th> 
        <th>Asychronous </th>
        <th>Technos</th>
        <th>Pros&nbsp;&amp;&nbsp;cons</th>
        <th>Resources</th>
    </thead>

    <tbody>



        <tr>
            <td>
                <a href="https://www.gravitee.io" target ="_blank">Gravitee</a><br/>
                <br/>
                <a href="../apim-gravitee/" target="_blank">Installation notes & test</a>
                
            </td>
            <td>APIM, Gateway, Developer portal </td>
            <td>
                <a href="https://documentation.gravitee.io/platform-overview/gravitee-essentials/gravitee-offerings-ce-vs-ee)" target="_blank">Limited community Edition</a>
            </td>
            <td>
                <ul>
                    <li>Scalability</li>
                    <li>Graphical&nbsp;designers&nbsp;for&nbsp;API&nbsp;and&nbsp;Policies</li>
                    <li>API lifecycle</li>
                    <li>Securization, OIDC Integration</li>
                    <li>Policies: request transformation, rate limits, etc.</li>
                    <li>Load balencer, failback health check</li>
                    <li>Dashboards, logs, alerts</li>
                </ul>
            </td>
            <td>
            <ul>
            <li>REST</li>
            <li>SOAP</li>
            <li>WebSocket</li>
            <li>Brockers</li>
            <li>GraphQL</li>
            </ul>
            </td>
             <td>
            <a href="https://www.gravitee.io/api-management-buyers-guide-event-native?_gl=1*605tnq*_up*MQ..&gclid=CjwKCAjwyY6pBhA9EiwAMzmfwQgPnBe3CrQurKvgBmeFASOJcg0U_0cOXzgat1EWxwUmzActDzXoZhoCtEMQAvD_BwE" target="_blank">YES</a>
            </td>
            <td>
                Java, Typescript, Angular
            </td>
            <td>
                <strong class="blue-text">Pros:</strong>
                <ul>
                <li>French product</li>
                <li>Developed in Java</li>
                <li><a href="https://github.com/gravitee-io/gravitee-api-management" target="_blank">Open source</a></li>
                <li>Good UI</li>
                <li>Monitoring (first impression)</li>
                </ul>
                <strong class="blue-text">Cons:</strong>
                <ul>
                <li>CE too limided for production</li> 
                <li>Expensive Licence for the EE</li>
               
                </ul>
            </td>
           
         

            <td>
            <ul>
            <li> <a href="https://documentation.gravitee.io/apim/overview/readme"  target="_blank">gravitee.io</a> </li>
            <li> <a href="https://documentation.gravitee.io/apim/overview/apim-architecture" target="_blank">APIM Achitecture</a> </li>
            <li> <a href="https://www.gravitee.io/api-management-buyers-guide-event-native?utm_term=best%20api%20management%20software&utm_campaign=S_NA_API+Management_EN&utm_source=google&utm_medium=cpc&hsa_acc=4006284794&hsa_cam=19928082466&hsa_grp=148280963992&hsa_ad=653420796429&hsa_src=g&hsa_tgt=kwd-490946531184&hsa_kw=best%20api%20management%20software&hsa_mt=p&hsa_net=adwords&hsa_ver=3&gclid=CjwKCAjw4P6oBhBsEiwAKYVkq86dix5csOo6TbPtgcmM4nh9h3EgHUfZVIxxRijDoxHLe3hMSMJipBoCVEQQAvD_BwE"  target="_blank">Gravitee Event-native API Management</a></li>
            <li><a href="https://github.com/gravitee-io/gravitee-api-management" target="_blank">GitHub repository</a></li>
            </ul>
            </td>
        </tr>

        <tr>
            <td>
                <a href="https://apisix.apache.org/downloads/" target="_blank">Apache APISIX</a><br/>
                <br/>
                <a href="../apim-apisix/" target="_blank">Installation&nbsp;notes&nbsp;&amp;&nbsp;test</a>
                </td>
            <td>APIM, Gateway <a href="https://apisix.apache.org/docs/apisix/3.1/tutorials/manage-api-consumers/#apache-apisix-consumer-example" target="_blank">(no&nbsp;developer&nbsp;portal?)</a></td>
            <td> Apache Software Foundation(ASF)'s open source project</td>
            <td>
                <ul>
                    <li>Scalability</li>
                    <li>UI or interract directly with APISIX API</li>
                   
                    <li>Securization, OIDC Integration, a lot of plugins</li>
                    <li>Request transformation, rate limits, etc.</li>
                    <li>Load balencer, failback health check</li>
                    <li>Dashboards <a href="https://grafana.com/oss/grafana/" target="_blank">(It uses Grafana)</a>, logs</li>
                </ul>
            </td>
            <td>   
                <ul>
                    <li>REST</li>
                    <li>SOAP</li>
                    <li>WebSocket</li>
                    <li>Brockers</li>
                    <li><a href="https://apisix.apache.org/blog/2022/03/02/apisix-integration-graphql/">GraphQL</a></li>
                    <li><a href="https://docs.api7.ai/apisix/key-concepts/stream-routes">Stream</a></li>
                </ul>
            </td>
            <td><a href="https://apisix.apache.org/blog/2022/11/07/webhook-api-gateway-event-driven-apis/" target="_blank">YES</a></td>
            <td><a href="https://apisix.apache.org/blog/2021/08/25/why-apache-apisix-chose-nginx-and-lua/" target="_blank">NGinx & LUA</a></td>
            <td>
             <strong class="blue-text">Pros:</strong>
                <ul>
                <li><strong>Apache&nbsp;Software&nbsp;Foundation:</strong><br/>full open source, minmal risk of licence change.</li>
                <li>Good UI ffor the APIM</li>
                <li>Independent dashboard and visualisation tool: <a href="https://grafana.com/oss/grafana/" target="_blank">grafana</a></li>
                <li>Based on Nginx</li>
                <li>A lot of plugins (and extensible via this mechanism)</li>
                <li>Good documentation</li>
                </ul>
                <strong class="blue-text">Cons:</strong>
                <ul>
                <li>Based on LUA</li>
                <li>No developer portal (not really a problem in our context?)</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li><a href="https://apisix.apache.org/" target="_blank">Apache&nbsp;APISIX&nbsp;Project</a></li>
                    <li><a href="https://opensource.com/article/23/1/api-gateway-apache-apisix" target="_blank">opensource.com</a></li>
                    <li><a href="https://github.com/apache/apisix" target="_blank">GitHub repository</a></li>
                </ul>
            </td>
        </tr>
        <tr>
            <td><a href="https://wso2.com/" target="_blank">WSO2</a></td>
            <td> 
                <ul>
                    <li><a href="https://apim.docs.wso2.com/en/latest/get-started/apim-architecture/" taget="_blank">APIM, Gateway, developer portal</a></li>
                    <li><a href="https://is.docs.wso2.com/en/latest/" taget="_blank">Identity and Access Management (IAM) </a></li>
                </ul>
            </td>
            <td>Yes, <a href="https://wso2.com/subscription/" target="_blank">subscription</a> seems to to be for support.</td>
            <td>Idem As Gravitee</td>
            <td>
                <a href="https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/deploying-apis-in-api-gateway-vs-choreo-connect/" target="_blank">
                    Two&nbsp;kind&nbsp;of&nbsp;gateways.
                </a>
             <ul>
                    <li>REST</li>
                    <li>SOAP</li>
                    <li>WebSocket</li>
                    <li>Brockers</li>
                    <li>GraphQL</li>
                    <li>Stream</li>
                </ul>
            </td>
            <td>
                <a href="https://apim.docs.wso2.com/en/latest/design/create-api/create-streaming-api/create-a-streaming-api-from-an-asyncapi-definition/" target="_blank">
                    YES
                </a>
            </td>
            <td> Java</td>
            <td> 
                <!-- <strong class="blue-text">Pros:</strong>
                <ul>
                    <li><a href="https://github.com/wso2/product-apim" target="_blank">Open source</a></li>
                </ul>

                <strong class="blue-text">Cons:</strong>
                <ul> 
            <li>XXX</li>
            </ul>-->
            </td>
            <td>
              <ul>
                    <li><a href="https://wso2.com/" target="_blank">wso2.com</a></li>
                    <li><a href="https://github.com/wso2" target="_blank">GitHub repository</a></li>
                </ul>
            </td>
        </tr>
        <tr>
            <td><a href="https://www.apiman.io/" taget="_blank">APIMAN</a></td>
            <td>APIM, Gateway, Developer portal </td>
            <td>
                Yes<br/>
                <a href="https://www.apiman.io/support.html" target="_blank"> Commercial Support</a>
            </td>
            <td>Idem As Gravitee</td>
            <td><ul>
                    <li>REST</li>
                    <li>SOAP</li>
                    <li>WebSocket (partially)</li>
                    <li>Brockers</li>
                    <li>GraphQL</li>
                    
                </ul></td>
            <td>Yes but only <a href="https://www.apiman.io/features.html" taget="_blank">partial support for Websockets</a></td>
            <td>Java & Typescript</td>
            <td></td>
            <td>
                <ul>
                    <li><a href="https://www.apiman.io/" target="_blank">www.apiman.io</a></li>
                    <li><a href="https://www.apiman.io/download.html" target="_blank">Quickstart</a></li>
                    <li><a href="https://github.com/apiman/apiman" target="_blank">GitHub repository</a></li>
                </ul>
              
            </td>
        </tr>

  <tr>
            <td><a href="https://konghq.com/" target="_blank">Kong</a></td>
            <td></td>
            <td><a href="https://docs.konghq.com/gateway/latest/kong-enterprise/" target="_blank">(Very) Limited Community Edition</a></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td><strong class="blue-text">Cons:</strong>
                <ul> 
            <li>Too limited CE, a lot of plugins and functionnalities require a subscription</li>
            <li>Pricing page not clear: seems to be about SASS mode.</li>
            </ul></td>
            <td>
                <ul>
                    <li><a href="https://konghq.com/" target="_blank">Homepage</a></li>
                    <li><a href="https://github.com/Kong/kong" target="_blank">GitHub repository</a></li>
                </ul>
            </td>
        </tr>
        <tr>
            <td><a href="https://www.krakend.io" target="_blank">KrakenD</a></td>
            <td></td>
            <td><a href="https://www.krakend.io/features/" target="_blank">Limited Community Edition</a></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td><strong class="blue-text">Cons:</strong>
                <ul> 
            <li>Too limited CE (missing basic auth & API keys for instance)</li>
            <li>Pricing unknown need to contact them</li>
            </ul></td>
            <td>
                <ul>
                    <li><a href="https://www.krakend.io/" target="_blank">www.krakend.io</a></li>
                    <li><a href="https://github.com/krakend/playground-community" target="_blank">KrakenD Playground</a></li>
                    <li><a href="https://github.com/krakend/krakend-ce" target="_blank">GitHub repository</a></li>
                </ul>
            </td>
        </tr>
        <tr>
            <td><a href="https://docs.solo.io/gloo-edge/latest/" target="_blank">GlooEdge</a></td>
            <td>Gateway</td>
            <td><a href="https://www.solo.io/wp-content/uploads/2021/06/Solo_Edge_Feature_Comparison_061621_v3.pdf" target="_blank">Limited Community Edition</a>
            </td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td>
             <ul>
                    <li><a href="https://www.solo.io/products/gloo-gateway/" target="_blank">solo.io</a></li>
                    <li><a href="https://apim.docs.wso2.com/en/latest/get-started/api-manager-quick-start-guide/" target="_blank">Quick start</a></li>
                    <li><a href=" https://github.com/solo-io/gloo" target="_blank">GitHub repository</a></li>
                </ul>
            
           </td>
        </tr>


    </tbody>
</table>

<br/>

## Rejected solutions
<table>
    <thead>
        <th>Name</th>
        <th>Justification</th>
    </thead>
    <tbody>
        <tr>
            <td><a href="https://azure.microsoft.com/fr-fr/products/api-management" target="_blank">Azure APIM</a></td>
            <td>SASS Only</td>
        </tr>
         <tr>
            <td><a href="https://cloud.google.com/apigee" target="_blank">Apigee (Google)</a></td>
            <td>SASS Only</td>
        </tr>
         <tr>
            <td><a href="https://aws.amazon.com/fr/api-gateway/" target="_blank">Amazon API Gateway</a></td>
            <td>SASS Only</td>
        </tr>
          <tr>
            <td><a href="https://aws.amazon.com/fr/api-gateway/" target="_blank">IBM API Connect </a></td>
            <td> <a href="https://www.ibm.com/products/api-connect/pricing">Pricing</a></td>
        </tr>
        <tr>
            <td><a href="https://tyk.io/" target="_blank">Tyk</a></td>
            <td><a href="https://tyk.io/pricing-self-managed/">The pricing policy does not seem to be adapted (to be confirmed)</a></td>
        </tr>
        <!-- <tr>
            <td><a href="https://konghq.com/" target="_blank">Kong</a></td>
            <td><a href="https://konghq.com/pricing" target="_blank">Pricing policy: Pay-as-you-go (need to be confirmed, perhaps only for the SASS version)</a></td>
        </tr> -->
        <tr>
            <td><a href="https://www.mulesoft.com/platform/api-management" target="_blank">Mulesoft</a></td>
            <td>SASS Only</td>
        </tr>
        <tr>
            <td><a href="https://traefik.io" target="_blank">Traefik</a></td>
            <td>SASS Only</td>
        </tr>
       
        <tr>
            <td><a href="https://www.postman.com" taget="_blank">Postman</a></td>
            <td><a href="https://www.postman.com/pricing/" target="_blank">Pricing policy</a></td>
        </tr>
        <tr>
            <td><a href="https://apidog.com" target="_blank"> Apidog</a></td>
            <td><a href="https://apidog.com/pricing/" target="_blank">Pricing policy</a></td>
        </tr>
        <tr>
            <td><a href="https://www.fusio-project.org/download" target="_blank">Fusio</a></td>
            <td><a href="https://github.com/apioo/fusio" target="_blank">Too low community ? (3 contributors only)</a></td>
        </tr>
        <tr>
            <td><a href="https://www.fusio-project.org/download" target="_blank">Boomi</a></td>
            <td><a href="https://boomi.com/fr/platform-services-and-pricing/" target="_blank">Subscription-based pricing is based on the number of API calls</a></td>
        </tr>
         <tr>
            <td><a href="https://api-umbrella.readthedocs.io/en/latest/index.html" target="_blank">Api Umbrella</a></td>
            <td><a href="https://api-umbrella.readthedocs.io/en/latest/api-consumer/api-key-usage.html" target="_blank">Only API Key (not OAuth/OIDC)</a></td>
            <td><a href="https://github.com/NREL/api-umbrella/issues/202" target="_blank">See this issue too</a></td>
        </tr>
    </tbody>
</table>




