---
layout: page
title: WebSockets and SSE  for communication microservice
permalink: /adr-websockets-and-sse/
up: ../adl/
---


## Status

Status: Accepted
Date: 2025-04-03

## Context

The [communication module](../arch-soft-specif-communication){:target="_blank"} need a realtime feature for 
- The notifications,
- Building reactive UI, e.g. instant messaging for instance.
- System messages, e.g. updating granted roles to users.

### Pros & Cons
- The Server-Sent Events (SSE) protocole is a lightweight, unidirectional http based protocol. 
- It is well suited when going through proxies, firewalls, or API gateways.
- Generally considered less resource-efficient than WebSockets on the backend side.
- Websockets can be blocked by firewalls. The fallback to SSE could be a workaround.
- SSE is heavier than websockets.
- SSE is unidirectional.
- Some functionalities are not available with SSE, e.g. rooms

## Decision

The decision is to **use WebSockets and Server-Sent Events (SSE) as a fallback** for the communication microservice.

**The strategy could be this one:**
Websockets (nominal case) -> SSE -> Short pooling -> asynchrone.


## Consequences

The choice of WebSockets and SSE has the following consequences:
- Two implementations are needed.
- The client need to handle both protocols.
- SSE is currently not supported by APISIX, which may require a workaround or future adaptation



