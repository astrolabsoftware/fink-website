---
layout: post
date: 2023-01-25
title: Fink is expanding!
author: fink
tags: [release]
---

The Fink broker release 2.9 includes new science modules exploring anomaly detection, time-series transformer for transient classification, and additional associations to various catalogs of astronomical objects. Additionally, the Fink Science Portal release 2.1 adds a lot of new features!
<!--more-->

## Crossmatch with catalogs

### AGN

Each alert packet includes now the closest match to the [4LAC DR3](https://fermi.gsfc.nasa.gov/ssc/data/access/lat/4LACDR3/) and [3HSP](https://www.ssdc.asi.it/3hsp/) catalogs, if it exists within 1 arcminute. These catalogs contain Active Galactic Nuclei detected by the LAT (4LAC) based on 8 years of data, and extreme and high-synchrotron peaked blazars and blazar candidates (3HSP).

### Gravitational waves

Each alert packet includes now the closest match to the [Mangrove](https://mangrove.lal.in2p3.fr) catalog of galaxies, if it exists within 1 arcmin. Mangrove has been designed for the follow up of gravitational waves events, and it contains curated and augmented information of both GLADE and AllWISE catalogs.

## Anomaly detection

Perhaps one of the most anticipated outcomes of the Rubin era is the identification of unknown astrophysical sources. As an example, potentially cataclysmic events leading to new mechanisms of particle acceleration or electromagnetic counterparts of GW can provide important insights into their progenitor systems. However, given the volume and complexity of the data which will accompany them, it is unlikely that such sources will be serendipitously identified. We need to develop tailored machine learning algorithms to detect such interesting candidates and direct them to the experts who can give meaning to a new discovery!

We are happy to announce that Fink now also provides results from an anomaly detection module. It was constructed aiming to identify statistically anomalous alerts without a cataloged cross-match. This is the first milestone for Fink in the realm of anomaly detection. However, we do not expect that this module will grow and evolve with time, in terms of the machine learning methods it uses, but also in incorporating feedback from the community. Thus, do not hesitate to give us your feedback on it.

## Transformers

Following the work of [Allam et al 2021](https://arxiv.org/abs/2105.06178), we deployed an optimised version of the original time-series transformer, `t2`. This classifier has been trained to target a large variety of transients, from galactic to extra-galactic.

## How to use the output of these modules?

As all the other modules, the output of these are publicly available (see [here](https://fink-broker.readthedocs.io/en/latest/science/added_values/) their description). You can access it via all Fink services: Livestream, Data Transfer, and Science Portal. Note that previous alerts are not reprocessed, and these new science modules start acting from 2023/01/25.

## What is changing for users in Fink 2.9?

While most changes are transparent to the users, we changed the way we handle alert schema in the Livestream service, and users need to upgrade their client to keep polling alerts:

```python
# 4.0+ required
pip install fink-client --upgrade
```

## Fink Science Portal 2.1

In this new release ([release notes](https://github.com/astrolabsoftware/fink-science-portal/releases/tag/2.1)), not only bugs have been fixed and layout improved, but new features have been added. Among several, a new lightcurve model for Solar System objects based on their spin has been added. This work is supported by the MITI grant (CNRS), and a paper summarising the findings is ongoing. Stay tuned!

## Thanks

Fink is a community-driven broker, and this work would not be possible without all the science teams behind the design and implementation of all these science modules. Particular thanks to
- Anomaly detection: Igor, Maria
- 4LAC, 3HSP: Jonathan, Jean-Philippe
- T2: Tarek
- Mangrove: the GRANDMA team, Sarah, Theophile, Michael, and all the observers!
- Fink Science Portal: Sergey, Quentin, Max, Benoit, Jerome