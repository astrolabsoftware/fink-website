---
layout: post
date: 2022-01-18
title: Interface changes
author: julien
tags: [API]
---

## API changes

With the recent maintenance, we did a number of changes to sanitize the infrastructure and the pipelines. Some of these changes breaks the compatibility with older Fink versions. All changes apply from `fink-broker` version 1.4+ and `fink-science` version 0.5+.

### Active Learning module

We introduced a name change for the output of the module. The old `rfscore` field is now replaced by `rf_snia_vs_nonia`. The module itself did not change, and the values remain the same.

### Kilonova module

We introduced a name change for the output of the module. The old `knscore` field is now replaced by `rf_kn_vs_nonkn`. The module itself did not change, and the values remain the same.

### Microlensing module

We shipped a new microlensing module still based on LIA, with a new schema for the output. We are not outputing anymore a `struct` with classes and probabilities from LIA, but a single `double` that is the mean of the per-band probabilities.

### REST API

#### Queries

Nothing changed from the end-user perspective in the REST API. Indeed, we combine internally science module outputs in Fink to provide with you alert classification. So despite the active learning score `rfscore` changed its name into `rf_snia_vs_nonia`, the Early SN Ia candidates have still the label `Early SN Ia candidate`.

#### Data columns

The data columns have their names changed as well:

- `d:rfscore` becomes `d:rf_snia_vs_nonia`
- `d:knscore` becomes `d:rf_kn_vs_nonkn`
- `d:mulens_*` becomes `d:mulens`, and it is a `double`


All documentation has been 