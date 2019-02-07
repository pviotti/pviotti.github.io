---
layout: post
title:  "PhD Weekly report"
date:   2015-11-06 18:00:00
categories: report
---


Explored the area of Declarative programming for consistency.
Written research proposal on declarative rogramming for consistency in distributed storage systems.

Started editing the presentation for CloudSpaces Brussels meeting.

Added a couple of references to papers by Golab to the survey.

## Papers read

 - Lasp: a programming language for distributed coordination-free programming
 - Declarative programming over eventually consistent data stores
 - Client-centric benchmarking of eventual consistency for cloud storage systems

## Notes on *"Declarative programming over eventually consistent data stores"*

They propose, implement and evaluate a declarative programming model for specifying 
and enforcing fine-grained transactional and non-transactional consistency semantics on top of e.c. stores.


Generic notes:

 - 3 people involved: 1 prof, 1 post-doc, 1 PhD - Purdue (US)
 - the first commit of source code dates back to Feb 2014.
 - the model uses concepts similar or taken from Burckhardt's work (though they cite as 'inspiring' the POPL'14 paper, not the book), except:
	* there is no ar: all conditions are just based on vis (also because it's mostly evaluated from client perspective, I guess)
	* the relations are among effects of operations, rather than operations
	* the state is a sequence of (immutable) effects, and performing operations amounts to reducing over the set of effects (which also entails conflict resolution)


Strenghts:

 - allows definition of customized RDTs
 - application-level consistency constraints, declaratively specified by the programmer, 
 are analysed and mapped to a store-level consistency property at compile time (by the Z3 theorem prover) (no run-time overhead)
 - *"Quelea treats the convergence semantics (i.e., how conflicting updates are resolved) of the data type separately from 
 its consistency properties (i.e., when updates become visible). This separation of concerns permits operational reasoning for conflict resolution, and declarative reasoning for consistency"*
 - support for coordination-free transactions (MAV, RC, RR isolation levels)
 - thorough and well-documented evaluation (EC2, with geo-replication too; many example applications)
 - open source, using Haskell and Cassandra (just as a backing store): http://gowthamk.github.io/Quelea/
 - (nice topic overview in the related work section)


Weaknesses:
 
 - their definitions of consistency models are not precise (EC, RYW), limited (CC) or downright wrong (MW; MR; "strong consistency", just as a bland constraint on visibility, no ordering whatsoever..)
    * possible major cause: there is no notion of explicit ordering of operations or transactions (no ar)
 - their definitions of transactional isolation levels are flawed as well
 - no support for staleness based models (requires some kind of wall clock, or version tracking)
 - sometimes they use syntax not explained nor referenced which I don't fully understand



## What is your plan for next week?

Finish CloudSpaces presentation

Continue exploring declarative consistency
