---
layout: post
title: Monolithic applications are not so bad actually
---

Monolithic applications have a lot of advantages over microservices ones. For example:
 
- Strong reference check accross the whole system.   
- Tooling and compiler support for system wide refactoring
- Easy debugging 
- Easy to trace through call chains to understand system behavior

       
# What might be bad  
- difficult to enforce architecture discipline(e.g. horizontal partitioning). Withing a monolithic application, horizontal partitioning needs human discipline, it would be a lot easier if reference between partitions are technically not possible.
- difficult to adopt different technology stack
- difficult to adopt different architecture design in different part of system. 
- difficult to scale
