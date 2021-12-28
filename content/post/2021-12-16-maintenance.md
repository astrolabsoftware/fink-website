---
layout: post
date: 2023-12-16
title: Fink maintenance
author: julien
tags: [maintenance]
---

## Power outage strikes back

This year, we had regular power outages at the VirtualData center, mainly due to a weak and old electrical network at the University. The IT teams at IJCLab regularly restarted all machines and services in a short amount of time after hard cuts (included evenings and week-ends!), and we designed more and more robust and automatic systems to keep the whole infrastructure healthy and operative after going completely crazy. But our systems are _a priori_ not designed to endure repeated (and random) blackouts, and the last problem (14/12) was fatal to the storage system part.

Long story short, a large part of Fink data cannot be recovered, making difficult to get some services back online, and we had no backup for this. It is yet unclear what happened exactly, and a post-mortem analysis is ongoing.

Only the database part is affected (serving the Science Portal), and the time-critical part (that processes nightly data) will continue to run and process new data as it comes during this maintenance. 

We will take the opportunity of this maintenance to learn lessons from the post-mortem analysis, refine the roadmap for the infrastructure, consolidate the weak points, and deploy new services that were on tests. We will get back stronger than ever!

We cannot predict when services will get back online with full capabilities. Fortunately raw data is accessible, and all previous Fink data can be reconstructed. You will be notified of any updates.

We apologize for the inconvience.

Julien


### What you should expect in 2022

The University has finally planned work to improve the electrical network. The work should start during the Christmas break, and we hope that the situation will be better from 2022. On a more optimistic note, 2022 will also be the time when Fink will start the deployment of its production platform at the CC-IN2P3 data center. CC has a long tradition of stability, and we are confident that Fink will be able to operate in a very stable environment during Rubin operations.
