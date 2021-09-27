---
layout: post
date: 2021-09-27
title:  Fink Science Portal 0.10
author: fink
tags: [Portal]
---

Check out the version 0.10 of the Science Portal! Several new features: DNS for the portal, crossmatch with LIGO/Virgo maps, query string, and as always... a lot of bug fixes!
<!--more-->

## DNS for the portal: https://fink-portal.org

So far, you were connecting to the Science Portal at 134.158.75.151:24000. I've heard that IP addresses are so 1980s, and http is so 1990s... So here we are, a brand new domain name, and a secured connection! We even add a reverse proxy! Christmas before Christmas -- enjoy https://fink-portal.org !

_Note that the old link is still valid to give you the time to transition._

## Crossmatch with LIGO/Virgo maps

Let's assume you want get all alerts falling inside a given LIGO/Virgo credible region sky map
(retrieved from the GraceDB event page, or distributed via GCN). You would
simply upload the sky map with a threshold, and Fink returns all alerts emitted
within `[-1 day, +6 day]` from the GW event inside the chosen credible region.
Concretely on [S200219ac](https://gracedb.ligo.org/superevents/S200219ac/view/):

```python
# LIGO/Virgo probability sky maps, as gzipped FITS (bayestar.fits.gz)
# S200219ac on 2020-02-19T09:44:15.197173
fn = 'bayestar.fits.gz'

# GW credible region threshold to look for. Note that the values in the resulting
# credible level map vary inversely with probability density: the most probable pixel is
# assigned to the credible level 0.0, and the least likely pixel is assigned the credible level 1.0.
# Area of the 20% Credible Region:
credible_level = 0.2

# Query Fink
data = open(fn, 'rb').read()
r = requests.post(
    'http://134.158.75.151:24000/api/v1/bayestar',
    json={
        'bayestar': str(data),
        'credible_level': credible_level,
        'output-format': 'json'
    }
)

pdf = pd.read_json(r.content)
```

You will get a Pandas DataFrame as usual, with all alerts inside the region (within `[-1 day, +6 day]`).

![gw](https://user-images.githubusercontent.com/20426972/134175884-3b190fa9-8051-4a1d-8bf8-cc8b47252494.png)

More information at https://fink-portal.org/api. Note this feature is available only from the REST API (not available from the website).

## Query String

You can now query Fink database directly from an URL. You have to specify two things:

- The type of query `query_type=`. Here, choose among `objectID`, `Conesearch`, `Date Search`, `Class Search`, `SSO Search`.
- The list of parameters to pass and their value `param=value`, separated by `&`. The parameters are the same as the API (see https://fink-portal.org/api).

See below some concrete examples.

### Object ID search

```
https://fink-portal.org/?query_type=objectID&objectID=ZTF21abfmbix
```

### Conesearch

```
https://fink-portal.org/?query_type=Conesearch&ra=193.822&dec=2.89732&radius=5&startdate_conesearch=2021-07-01 05:59:36&window_days_conesearch=1
```

Note that `startdate_conesearch` and `window_days_conesearch` are optional.

### Date search

```
https://fink-portal.org/?query_type=Date Search&startdate=2020-07-01%2005:59:36&window=1
```

### Class search

```
https://fink-portal.org/?query_type=Class Search&class=Early SN Ia candidate
```

### Solar system object search

```
https://fink-portal.org/?query_type=SSO Search&n_or_d=4209
```

## Full changelog

[https://github.com/astrolabsoftware/fink-science-portal/milestone/11?closed=1](https://github.com/astrolabsoftware/fink-science-portal/milestone/11?closed=1)
