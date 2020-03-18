---
layout: page
title: About
permalink: /about.html
feature-img: "assets/img/pexels/circuit.jpeg"
tags: [About, Archive]
---

Fink is a broker infrastructure enabling a wide range of applications and services to connect to large streams of alerts issued from telescopes all over the world. Fink core is based on the [Apache Spark](http://spark.apache.org/) framework, and more specifically it uses the [Structured Streaming processing engine](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html). The language chosen for the API is Python, which is widely used in the astronomy community, has a large scientific ecosystem and easily connects with existing tools.

Fink's goal is twofold: providing a robust infrastructure and state-of-the-art streaming services to LSST scientists, and enabling user-defined science cases in a big data context. Fink decouples resources needed for listening to the stream (online, critical), and resources used for services: scalable, robust, and modular!

We want Fink to be able to _filter, aggregate, enrich, consume_ incoming data streams or otherwise _transform_ into new streams for further consumption or follow-up processing. Following LSST [LDM-612](https://github.com/lsst/LDM-612), Fink's ultimate objectives are (no specific order):

* redistributing alert packets
* filtering alerts
* cross-correlating alerts with other static catalogs or alert stream
* classifying events scientifically
* providing user interfaces to the data
* coordinating scientific activity among collaborators
* triggering followup observing
* for users with appropriate data rights, facilitating followup queries and/or user-generated processing within the corresponding Data Access Center
* managing annotation & citation as followup observations are made
* collecting classification and other information gathered by the scientific community

## Getting started

Learning Fink is easy whether you are a developer or a scientist:

* Learn about the [broker technology](https://fink-broker.readthedocs.io/en/latest/broker/introduction/), the [science](https://fink-broker.readthedocs.io/en/latest/science/introduction/) we do, and how to [receive](https://fink-broker.readthedocs.io/en/latest/fink-client/) alerts.
* Learn how to use the broker or how to contribute following the different [tutorials](https://fink-broker.readthedocs.io/en/latest/tutorials/introduction/).
* Explore the different components:
    * [fink-alert-simulator](https://github.com/astrolabsoftware/fink-alert-simulator): Simulate alert streams for the Fink broker.
    * [fink-broker](https://github.com/astrolabsoftware/fink-broker): Astronomy Broker based on Apache Spark.
    * [fink-science](https://github.com/astrolabsoftware/fink-science): Define your science modules to add values to Fink alerts.
    * [fink-filters](https://github.com/astrolabsoftware/fink-filters): Define your filters to create your alert stream in Fink.
    * [fink-client](https://github.com/astrolabsoftware/fink-client):  Light-weight client to manipulate alerts from Fink.
