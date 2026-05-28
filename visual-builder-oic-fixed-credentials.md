---
layout: article
title: Using fixed credentials from Oracle VB to OIC? Think again
description: Why fixed Visual Builder credentials can overexpose backends.
date: 2026-05-28
topic: Oracle Visual Builder and OIC
article: true
---

## The insecure architecture
The following architecture introduces serious security vulnerabilities:
- A system user, or confidential application, having the Service Invoker role for your Oracle Integration Cloud instance
- A Visual Builder backend pointing to the OIC instance using fixed credentials (e.g., basic authentication, or OAuth client credentials)
- A Visual Builder service connection using this backend

## Why this architecture is insecure
This architecture effectively gives **any authenticated user** the ability to invoke **any integration** on your OIC instance:
- even if you have Role-Based Access Control on your Visual Builder web application
- even if your Visual Builder backend URL is specific to an OIC project/integration

## Root-cause of the security issue
As of the time of writing this article:
- Oracle Visual Builder exposes all service connections to an API that can be accessed by any authenticated user unless you make it a "server-only connection".
- OIC does not have runtime Role-Based Access Control per integration

## Alternatives

### Visual Builder Business Object proxy + server-only service connection
Mark the OIC service connection as "server-only", and use Business Object functions with Role-Based Access Control to access it.

### Other proxy with identity propagation
Instead of creating a service connection to OIC:
- create a service connection to some proxy (e.g., OCI API gateway, or Oracle Fusion Application Composer custom object function)
- propagate the identity to this proxy (e.g., Oracle Cloud Account authentication)
- implement RBAC in this proxy
