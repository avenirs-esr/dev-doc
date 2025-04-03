---
layout: page
title: SSE preferred over WebSockets for communication microservice
permalink: /adr-sse-versus-websockets/
up: ../adl/
---


## Status

Status: Accepted
Date: 2025-04-03

## Context

The [communication module](../arch-soft-specif-communication){:target="_blank"} need a realtime feature for 
- The notifications,
- Building reactive UI, e.g. instant messaging for instance.
- System message, e.g. updating granted roles to users.


- The Server-Sent Events (SSE) protocole is a lightweight, unidirectional http based protocol. 
- It is well suited when going through proxies, firewalls, or API gateways.
- Generally considered more resource-efficient than WebSockets on the backend side.

## Decision

The decision is to **use Server-Sent Events (SSE)** for the communication microservice.


## Consequences

The choice of SSE following consequences:
- Simpler and lighter implementation (base http protocol).
- The rooms will have to be implemented.
- Unidirectional: no feature like "user is typing".
- SSE is currently not supported by APISIX, which may require a workaround or future adaptation



