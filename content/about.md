---
title: About
permalink: /about.html
tags: [About, Archive]
---

Fink is a community driven project, open to anyone, that processes time-domains alert streams and connects them with follow-up facilities and science teams. Fink broker has been selected as a community broker to process the full stream of transient alerts from the [Vera C. Rubin Observatory](https://lsst.org/). Since 2020, we are processing the alert stream from the [Zwicky Transient Facility](https://www.ztf.caltech.edu/) (ZTF).

Fink's processed data stream from ZTF can be accessed through our [science portal](https://fink-portal.org) and our [API](https://fink-portal.org/api) (with [tutorials](https://github.com/astrolabsoftware/fink-notebook-template)). This data is aggregated at the end of every observing night by ZTF. For automatic filtered data streams within minutes of observations, please contact us at contact@fink-broker.org An overview of Fink broker architecture and first science results can be found in our white paper published in [MNRAS](https://academic.oup.com/mnras/article/501/3/3272/5992334) and [arxiv](https://arxiv.org/abs/2009.10185).

Fink is currently deployed in Université Paris Saclay thanks to the contribution of IJCLab. For Rubin LSST Fink will be deployed at CC-IN2P3 scientific data centre. 

## About Fink's infrastructure

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

## About Fink's collaboration
We are eager to have a vibrant collaboration with members from both engineering and research backgrounds around the world. We are commiteed to have an inclusive and respectful collaboration. Our Code of Conduct is available [here](https://drive.google.com/file/d/1U3nhLDYkAbaxD3dszvQPflkDlM2s1X6-/view?usp=share_link).

## Getting started

If you want to join Fink please check our [Joining page](https://fink-broker.org/joining/), and you will find the documentation website at [https://fink-broker.readthedocs.io](https://fink-broker.readthedocs.io).

