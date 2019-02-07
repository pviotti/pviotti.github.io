---
layout: post
title:  "PhD Weekly report"
date:   2015-12-04 18:00:00
categories: report
---

Submitted the survey both to journal and to ArXiv.  
Corrected the paper to comply with journal's anti-plagiarism restrictions.   

(from "A deep specification for Dropbox")

 * induce operation events ( E )
 * observe outcomes of events ( H=(E,op,rb,ss,ob) )
 * add "invisible operation" to justify observation ( vis, ar )
 * check whether the added "invisible operations" + the concrete outcomes match a predefined model (maybe defined os FSM) (validate consistency predicates)

Tool required:
 
 - QuickCheck to generate property-based tests
 - a key value store (Cassandra?)
 - a theorem prover / model checker to validate predicates against executions

So either, using a top-down approach, we embed formal specification into declarative programming languages,
or we can use those formal specification in a testing framework based on property-based testing.
 

## Papers read 

 - review of: FIT paper by Faleiro and Abadi, 2015
 - presentation on Chain Replication at Ricon 2015 by Scott Fritchie
 - A Deep Specification for Dropbox - [Benjamin Pierce](http://www.cis.upenn.edu/~bcpierce/) - [ClojureCon presentation](https://www.youtube.com/watch?v=Y2jQe8DFzUM)


## Plan for next week

Try to investigate the use of QuickCheck: write a first prototype.  
Scout theorem provers and formal tools.   

