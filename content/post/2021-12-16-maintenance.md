---
layout: post
date: 2021-12-16
title: Fink maintenance
author: julien
tags: [maintenance]
---

## Power outage strikes back

This year, we had regular power outages at the VirtualData center, mainly due to a weak and old electrical network at the University. The IT teams at IJCLab regularly restarted all machines and services in a short amount of time after hard cuts (included evenings and week-ends!), and we designed more and more robust and automatic systems to keep the whole infrastructure healthy and operative after going completely crazy. But our systems are _a priori_ not designed to endure repeated (and random) blackouts, and the last power outage was fatal to the storage system.

Long story short, a large part of Fink data was corrupted in a way that it is difficult to get services back online, and we had no backup for this. We found problems that started in the background after the previous power outage (05/11), and were amplified with the last outage.

Only the database part is affected (serving the Science Portal), and the time-critical part (that processes nightly data) will continue to run and process new data as it comes during this maintenance. I cannot predict when services will get back online with full capabilities, and you will be notified of any updates.

We will take the opportunity of this maintenance to refine the roadmap for the infrastructure in the light of the recent events, consolidate the weak points, and deploy new services that were on tests. We will get back stronger than ever!

Julien


### What you should expect in 2022

The University has finally planned work to improve the electrical network. The work should start during the Christmas break, and we hope that the situation will be better from 2022. On a more optimistic note, 2022 will also be the time when Fink will start the deployment of its production platform at the CC-IN2P3 data center. CC has a long tradition of stability, and we are confident that Fink will be able to operate in a very stable environment during Rubin operations.