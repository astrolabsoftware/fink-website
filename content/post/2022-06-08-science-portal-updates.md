---
layout: post
date: 2022-06-08
title: Fink Science Portal 1.2
author: fink
tags: [Portal]
---

Check out the version 1.2 of the Science Portal!
<!--more-->

## Add QR codes for objects

Now there is a QR code attached to each *object*. You will find it next to the name of the object in the summary page:

![qr_web](/images/qr_web.png)

Each QR code references to an object page `https://fink-portal/<objectId>`, so that you can easily switch from your laptop to smartphone, or use it in slides to share data with others! Try for example:

<img src="/images/qr_fink.png" width="30%" height="30%" />

## Enable ENTER key to send query

FINALLY there! I deeply apologise on this one... now you can write your query on the Fink search bar, and just press enter to execute it :-)

Note that the class search has been also automatised. Now it displays the 100 latest alerts
by default when you switch on the `Class Search`, and if you select a class from the list,
the query results will be automatically refreshed.

## Draw random objects from the database

We often want random objects. Now you can easily do this from the new API endpoint https://fink-portal.org/api/v1/random. This service lets you draw random objects (full light-curve) from the Fink database (120+ million alerts). This is still largely experimental. In python, you would use

```python
import io
import requests
import pandas as pd

r = requests.post(
  'https://fink-portal.org/api/v1/random',
  json={
    'n': integer, # Number of random objects to get. Maximum is 16.
    'class': classname, # Optional, specify a Fink class.
    'seed': integer, # Optional, the seed for reproducibility
    'columns': str, # Optional, comma-separated column names
    'output-format': output_format, # Optional [json[default], csv, parquet, votable]
  }
)

# Format output in a DataFrame
pdf = pd.read_json(io.BytesIO(r.content))
```

More information at https://fink-portal.org/api. Note that this is not available yet from the Science Portal (web), but there should be a page dedicated to this later.

## Limit the number of times an object is listed in the results

There is now a new button that gives the capability to group by alerts by name to only output a list of unique names (the last alert in time is kept):

![groupby](/images/group_objects.png)

Limitation: once the grouping by alert name is performed, one cannot go back to the full list (the table is overwritten). In order to get full results, the query needs to be re-ran.

Note that we also added two new columns to ease the search:
1. A new column `v:firstdate` which is the human readable datetime for the first variation of the object (from `i:jdstarthist`)
2. A new column `v:lapse` which is the number of days between first and last detection.

For example, one can easily then filter results by their age by adding `v:lapse` and sorting:

![lapse](/images/lapse.png)

The API is untouched, although new columns are available in the results (only when transferring all columns though).

## Querying the photometry for multiple objects at once

When you retrieve object data, you can now specify several object names at once:

```python
import io
import requests
import pandas as pd

mylist = ['ZTF21aaxtctv', 'ZTF21abfmbix', 'ZTF21abfaohe']

# get data for many objects
r = requests.post(
  'https://fink-portal.org/api/v1/objects',
  json={
    'objectId': ','.join(mylist),
    'output-format': 'json'
  }
)

# Format output in a DataFrame
pdf = pd.read_json(io.BytesIO(r.content))
```

you will get a single DataFrame, with all objects stacked.

## Solar system objects

In the release 1.1, we only allowed for querying asteroid ephemerides in Miriade -- and query for comets would miserably fail. This has been fixed!

## Speeding up Class Search

As is done with other services, you can now specify the fields to transfer when querying latest alerts for a given class:

```python
import io
import requests
import pandas as pd

# Get all classified SN Ia from TNS between March 1st 2021 and March 15th 2021
r = requests.post(
  'https://fink-portal.org/api/v1/latests',
  json={
    'class': '(TNS) SN Ia',
    'n': '1000',
    'columns': 'i:objectId,i:jd,i:magpsf',
    'startdate': '2021-03-01',
    'stopdate': '2021-03-15'
  }
)

# Format output in a DataFrame
pdf = pd.read_json(io.BytesIO(r.content))
```

## New output format: VOTable

Regarding VOTables, we have an experimental support:

```
wget "https://fink-portal.org/api/v1/explorer?ra=193.822&dec=2.89732&radius=5&output-format=votable" -O conesearch.xml
```

or in Python

```python
import io
import requests
from astropy.io import votable

r = requests.post(
  'https://fink-portal.org/api/v1/objects',
  json={
    'objectId': 'ZTF21aaxtctv',
    'output-format': 'votable'
  }
)

vt = votable.parse(io.BytesIO(r.content))
```

## Proper POST URL query

Why not trying:

```
# photometry & metadata
wget "https://fink-portal.org/api/v1/objects?objectId=ZTF21aaxtctv&output-format=json" -O ZTF21aaxtctv.json

# conesearch
wget "https://fink-portal.org/api/v1/explorer?ra=193.822&dec=2.89732&radius=5&output-format=json" -O conesearch.json

# class search
wget "https://fink-portal.org/api/v1/latests?class=Early SN Ia candidate&n=100&columns=i:objectId,i:magpsf,i:jd" -O snia.json

# etc... ;-)
```

## Full changelog

Thanks to @anaismoller, @MatSmithAstro, @fusroman, @BrentMiszalski for feedback!

[https://github.com/astrolabsoftware/fink-science-portal/releases/tag/1.2](https://github.com/astrolabsoftware/fink-science-portal/releases/tag/1.2)
