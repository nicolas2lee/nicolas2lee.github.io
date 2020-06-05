---
layout: post
title:  "Cloud native app best practice"
date:   2020-06-06 00:00:14 +0100
categories: cloud, cloud native, kubernetes, docker
---
According to differenet model in cloud (Iaas, Paas, Saas, Caas, Kaas), the nature and best practices are not always the same. 
This post focuses on the applications deployed on kubernets with docker image format. The different discplines are applicable for all different languages, 
here we will take java, spring boot stack as an example, since it is the stack which I used during renctly years. 
# General principles
## DDD (domain driven development)
Domain driven development is not only a good design for cloud application, but also for most software applications
## TDD, BDD & automatic test
## Design for failure & design for recovery 
## Externalization of configuration
## Docker best practice
## Logging 
## Supervison liveness é readness of application
## Resource management 

Noawdays, a lot of new technologies appear, simplify & resolve a lot of pain points of traditional software problems. For example:
Serverless, service mesh, open application model and etc.

# Service mesh
Service mesh has serval different implmentations like: linkerd, istio... Here we will focuse on istio, since large companies often choose istio 
because of google, IBM, Red hat commercail support of Istio.
