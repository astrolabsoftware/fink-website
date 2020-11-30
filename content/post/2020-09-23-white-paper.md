---
layout: post
date: 2020-09-23
title:  Fink white paper
author: fink
tags: [paper]
---

After a year of activities, the Fink team is proud to release its [first paper](https://arxiv.org/abs/2009.10185)! The team made of researchers and engineers worked restlessly to make Fink comes true. Congratulations to all!
<!--more-->

Fink comes to join a few other brokers currently operating on other experiments, such as the Zwicky Transient Facility (ZTF, Bellm et al. 2018) or the High cadence Transient Survey (HiTS, Förster et al. 2016). Among these are ALeRCE (Förster et al. 2020), Ampel (Nordin et al. 2019), Antares (Narayan et al. 2018), Lasair (Smith et al. 2019), MARS and SkyPortal (van der Walt et al. 2019). ZTF has the particularity to use an alert system design that is very close to the one envisioned by LSST (Patterson et al. 2019), hence allowing to prototype and test systems with the relevant features and communication protocols prior to the start of LSST operations. 

In this paper, we first showcase the different science cases used for our original developments: supernovae, multi-messenger astronomy (gamma-ray bursts, gravitational waves, high-energy neutrinos, tidal disruption events), microlensing, anomaly detection. We then summarise the design choices motivated by these science cases and the technological challenges of the LSST alert stream to maximise the scientific exploitation of the alert stream. We outline the architecture and current implementation, and we test our first science modules which filter the ZTF public stream providing with supernovae, gamma-ray burst, microlensing and asteroid candidates. More information at [https://arxiv.org/abs/2009.10185](https://arxiv.org/abs/2009.10185).

<img src="/images/footprint_ztf_fink.png" width="100%" height="100%" style="display: block; margin: auto;" />
_Footprint of the ZTF alert stream from November 2019 to June 2020 associated to a subset of transient types using current Fink science modules: confirmed and candidates Solar System objects (top-left blue), variable stars from the cross-match with the Simbad catalog (orange top-right), alerts matched to a galaxy in the Simbad catalog (green middle-left), supernovae type Ia candidates selected using SuperNNova (red middle-right), microlensing event candidates selected using LIA (purple bottom-left), and all 7,975,588 processed alerts by Fink that pass quality cuts (bottom-right). The Planck Commander thermal dust map (Akrami et al. 2018) is shown in the background for reference. All maps are in the Galactic coordinate system, with a healpix resolution parameter equal to Nside=128 (Gorski et al. 2005), except for alerts matching galaxies (green middle-left) where Nside=64 has been used to increase the readability._

We are currently working on developing more science modules and improving our current ones. We invite the community for new contributions on new and existing science cases ([join us!](https://fink-broker.org/joining.html)). Importantly, we are constructing web-interfaces and API services to enable a seamless user experience and enable automatising follow-up coordination with observational facilities and teams. Stay tuned!
