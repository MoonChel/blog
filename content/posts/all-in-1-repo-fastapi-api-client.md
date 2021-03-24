---
title: All in 1 repo - FastAPI & API client
description: Sharing data models between API client and API itself
date: 2021-03-23T23:00:00Z

---
## Motivation

Clients, which are used for communication between the micro-services, are typically written by ourselves. 

What are the drawbacks to this strategy:

1. Everyone is implementing their own clients and tests for them, creating a lot of duplicity in code. 
2. In case primary API is changed and we don't have clients versioned, they will start to fail.

## Solution

There is an elegant solution, which I implemented by using new library called [FastAPI](https://fastapi.tiangolo.com/ "https://fastapi.tiangolo.com/"). You can check the code [here](https://github.com/MoonChel/cars "https://github.com/MoonChel/cars"). The main idea is that we can share data models implementation between API and client and keep both of the in the one repository. 

How this will eliminate problems described in the Motivation part:

1. No code duplicity - client is written one alongside with API and tested
2.  Client versioning can be applied - by [https://libraries.io/pypi](https://libraries.io/pypi "https://libraries.io/pypi") or some other similar service
