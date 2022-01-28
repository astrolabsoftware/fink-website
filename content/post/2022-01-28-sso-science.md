---
layout: post
date: 2022-01-28
title: Solar System Science with Fink
author: fink
tags: [sso]
---

## Linking alerts to known moving objects

Each night, Fink receives alerts from the public survey of the Zwicky Transient Facility (ZTF). The alert stream primarily includes events from flux transients and variables, but it also contains a large fraction of Solar System objects (SSO). Alert candidates are typically linked to an object on the sky by a cone search at the position of the alert. While this allows us to easily retrieve the brightness variation of a static object on the sky over time, this does not work if one wants to study a moving object.

Fortunately for us, ZTF performs also a cross match with the Minor Planet Center archive, and attach the result to the alerts. We get mainly 3 additional fields: `ssnamenr` (the name of the nearest SSO within 30 arcsec, if any), `ssdistnr` (the distance to the nearest SSO within 30 arcsec, if any), `ssmagnr` (the magnitude of nearest known SSO within 30 arcsec, if any). While we could use this information as is, we slightly refine the search to link alerts to SSO objects.

First, a cross match radius of 30 arcsec is rather broad -- there are sky fields for which there could be many sources. So first we keep only alerts with a ZTF MPC match within 5 arcsec from an MPC object. Second, we make also a condition on the distance to the nearest Panstarrs object, which should be bigger than the distance to MPC object (to remove stellar contamination). In pseudo-code, and using ZTF alert field names, the complete series of cuts is:

```python
if ssdistnr is not None:
    # alerts should be less than 5'' away from MPC object
    f_distance1 = ssdistnr >= 0.0
    f_distance2 = ssdistnr < 5.0

    # Distance to Panstarrs object should be bigger than distance to MPC object
    f_relative_distance = (abs(distpsnr1) - ssdistnr) > 0.0

    # Not seen many times with the same objectId
    f_ndethist = ndethist <= 2

    mask_roid = f_distance1 & f_distance2 & f_relative_distance & f_ndethist
```

Among the hundreds of thousands alerts that we receive nightly, about 10 to 50% are flagged as known Solar System objects.

![sso-evolution](/images/sso-evolution.png)
_Cumulative number of alerts flagged as known Solar System objects from MPC, taken from https://fink-portal.org/stats. After 2 years of operations (Nov 2019 to December 2021, 523 observing nights), Fink has flagged more than 10 million alerts as known Solar System objects._

## Science portal: a shiny new interface

In order to explore these moving objects, you can use the Fink Science Portal.

### Searching for SSO

#### By number

If you already know a Solar System object name (IAU identifier), you can directly query for it:

![sso-number-1](/images/sso-number-1.png)
_How to search for SSO by number? **A:** Choose the SSO query. **B:** Enter a valid IAU number. **C:** hit search!_

You will have the list of alerts that correspond to this SSO number:

![sso-number-2](/images/sso-number-2.png)

Note that you can display the orbit made by the alerts by hitting the button `Sky map` above the table:

![sso-number-3](/images/sso-number-3.png)

#### By class

You can also request the latest 100 alerts received linked to known Solar System objects:

![sso-class-1](/images/sso-class-1.png)
_How to get the lastest 100 alerts received linked to SSO? **A:** Choose the `Class Search` query. **B:** Choose the `Solar System (MPC)` field in the dropdown. **C:** hit search!_

You will have the list of alerts that correspond to this SSO number:

![sso-class-2](/images/sso-class-2.png)
_Note that you can easily add extra columns to the table, like the IAU number for these moving objects._

### Inspecting data for an object

Once you have your table result, you can further inspect an object. For this, click on any alert of the table. A new page will open with the data for the alert, and go to the Tab `Solar System`.

#### Lightcurve

The first Tab, `Lightcurve`, will display the lightcurve made from all linked alerts:

![sso-lc](/images/sso-lc.png)

The top plot is the lightcurve for the 2 ZTF filter bands (g & r), as well as ephemerides data provided by the [Miriade ephemeride service](https://ssp.imcce.fr/webservices/miriade/api/ephemcc/). The plot on the bottom shows the residuals between observed and predicted magnitude as a function of the ecliptic longitude. The variations are most-likely due to the difference of aspect angle: the object is not a perfect sphere, and we are seeing its oblateness here. The solid lines are sinusoidal fits to the residuals. 

The card on the right displays parameters for this object from the MPC archive. You also have two links to the MPC and JPL websites for further inspection.

#### Astrometry

The `Astrometry` Tab displays the difference between the alert positions and the positions returned by the [Miriade ephemeride service](https://ssp.imcce.fr/webservices/miriade/api/ephemcc/).

![sso-astrometry](/images/sso-astrometry.png)


#### Phase curve

The `Phase curve` Tab allows to inspect the phase curve. By default, the data is modeled after the three-parameter H, G1, G2 magnitude phase function for asteroids from [Muinonen et al. 2010](https://doi.org/10.1016/j.icarus.2010.04.003). We use the implementation in [sbpy](https://sbpy.readthedocs.io/en/latest/sbpy/photometry.html#disk-integrated-phase-function-models) to fit the data.

![sso-phase-1](/images/sso-phase-1.png)

We propose two cases, one fitting bands separately, and the other fitting both bands simultaneously (rescaled):

![sso-phase-2](/images/sso-phase-2.png)

We also propose different phase curve modeling using the HG12 and HG models. Hit buttons to see the fitted values!

![sso-phase-3](/images/sso-phase-3.png)

### Using the REST API

#### By number

The list of arguments for retrieving SSO data can be found at https://fink-portal.org/api/v1/sso.
The numbers or designations are taken from the MPC archive.
When searching for a particular asteroid or comet, it is best to use the IAU number,
as in 8467 for asteroid "8467 Benoitcarry". You can also try for numbered comet (e.g. 10P),
or interstellar object (none so far...). If the number does not yet exist, you can search for designation.
Here are some examples of valid queries:

* Asteroids by number (default)
  * Asteroids (Main Belt): 8467, 1922
  * Asteroids (Hungarians): 18582, 77799
  * Asteroids (Jupiter Trojans): 4501, 1583
  * Asteroids (Mars Crossers): 302530
* Asteroids by designation (if number does not exist yet)
  * 2010JO69, 2017AD19, 2012XK111
* Comets by number (default)
  * 10P, 249P, 124P
* Comets by designation (if number does no exist yet)
  * C/2020V2, C/2020R2

Note for designation, you can also use space (2010 JO69 or C/2020 V2).

In a unix shell, you would simply use

```bash
# Get data for the asteroid 8467 and save it in a CSV file
curl -H "Content-Type: application/json" -X POST -d '{"n_or_d":"8467", "output-format":"csv"}' https://fink-portal.org/api/v1/sso -o 8467.csv
```

In python, you would use

```python
import requests
import pandas as pd

# get data for object 8467
r = requests.post(
  'https://fink-portal.org/api/v1/sso',
  json={
    'n_or_d': '8467',
    'output-format': 'json'
  }
)

# Format output in a DataFrame
pdf = pd.read_json(r.content)
```

Note that for `csv` output, you need to use

```python
# get data for asteroid 8467 in CSV format...
r = ...

pd.read_csv(io.BytesIO(r.content))
```

You can also get a votable using the json output format:

```python
from astropy.table import Table

# get data for asteroid 8467 in JSON format...
r = ...

t = Table(r.json())
```

You can also attach the ephemerides provided by the [Miriade ephemeride service](https://ssp.imcce.fr/webservices/miriade/api/ephemcc/):

```python
import requests
import pandas as pd

# get data for object 8467
r = requests.post(
  'https://fink-portal.org/api/v1/sso',
  json={
    'n_or_d': '8467',
    'withEphem': True,
    'output-format': 'json'
  }
)

# Format output in a DataFrame
pdf = pd.read_json(r.content)
print(pdf.columns)
Index(['index', 'Date', 'LAST', 'HA', 'Az', 'H', 'Dobs', 'Dhelio', 'VMag',
       'SDSS:g', 'SDSS:r', 'Phase', 'Elong.', 'AM', 'dRAcosDEC', 'dDEC', 'RV',
       'RA', 'Dec', 'Longitude', 'Latitude', 'd:cdsxmatch', 'd:mulens',
       'd:rf_kn_vs_nonkn', 'd:rf_snia_vs_nonia', 'd:roid', 'd:snn_sn_vs_all',
       'd:snn_snia_vs_nonia', 'i:candid', 'i:chipsf', 'i:classtar', 'i:dec',
       'i:diffmaglim', 'i:distnr', 'i:distpsnr1', 'i:drb', 'i:fid', 'i:field',
       'i:isdiffpos', 'i:jd', 'i:jdendhist', 'i:jdstarthist', 'i:maggaia',
       'i:magnr', 'i:magpsf', 'i:magzpsci', 'i:ndethist', 'i:neargaia',
       'i:nid', 'i:nmtchps', 'i:objectId', 'i:publisher', 'i:ra', 'i:rb',
       'i:rcid', 'i:sgscore1', 'i:sigmagnr', 'i:sigmapsf', 'i:ssdistnr',
       'i:ssmagnr', 'i:ssnamenr', 'i:tooflag', 'i:xpos', 'i:ypos',
       'd:tracklet', 'v:classification', 'v:lastdate', 'v:constellation',
       'i:magpsf_red'],
      dtype='object')
```

Where first columns are fields returned from Miriade (beware it adds few seconds delay).
By default, we transfer all available data fields (original ZTF fields and Fink science module outputs).
But you can also choose to transfer only a subset of the fields:

```python
# select only jd, and magpsf
r = requests.post(
  'https://fink-portal.org/api/v1/sso',
  json={
    'n_or_d': '8467',
    'columns': 'i:jd,i:magpsf'
  }
)
```

Note that the fields should be comma-separated. Unknown field names are ignored. More information at https://fink-portal.org/api.

#### By class

The list of arguments for getting latest alerts by class can be found at https://fink-portal.org/api/v1/latests.

The list of Fink class can be found at https://fink-portal.org/api/v1/classes

```bash
# Get list of available class in Fink
curl -H "Content-Type: application/json" -X GET https://fink-portal.org/api/v1/classes -o finkclass.json
```

To get the last 5 candidates of the class `Solar System MPC`, you would simply use in a unix shell:

```bash
# Get latests 5 Solar System MPC
curl -H "Content-Type: application/json" -X POST -d '{"class":"Solar System MPC", "n":"5"}' https://fink-portal.org/api/v1/latests -o out.json
```

In python, you would use

```python
import requests
import pandas as pd

# Get latests 5 Solar System MPC alerts
r = requests.post(
  'https://fink-portal.org/api/v1/latests',
  json={
    'class': 'Solar System MPC',
    'n': '5'
  }
)

# Format output in a DataFrame
pdf = pd.read_json(r.content)
```

You can also specify `startdate` and `stopdate` for your search:

```python
import requests
import pandas as pd

# Get all Solar System MPC alerts between March 1st 2021 and March 5th 2021
r = requests.post(
  'https://fink-portal.org/api/v1/latests',
  json={
    'class': 'Solar System MPC',
    'n': '100',
    'startdate': '2021-03-01',
    'stopdate': '2021-03-05'
  }
)

# Format output in a DataFrame
pdf = pd.read_json(r.content)
```
There is no limit of time, but you will be limited by the
number of alerts retrieve on the server side `n` (current max is 1000).

## Planned improvements

The service has some limitations, and here are the planned improvements for the coming months:

1. SSOs do not have unique names or identifiers usually... While we stick with IAU identifiers for the moment, we plan to use a low level name resolver ([quaero](https://ssp.imcce.fr/webservices/ssodnet/api/quaero/)) to use aliases as well.
2. For the moment, users can search by SSO number or do a `Class Search` to get latest alerts. Ideally, we would like the users to query also SSO by the number of time they have been seen in ZTF.

## How to receive alerts in real-time about an object?

The Science Portal databases are updated with new data only at the end of an observation night. If you want to receive alerts in real-time from Fink, you can design a [filter](https://github.com/astrolabsoftware/fink-filters) and use the [client](https://github.com/astrolabsoftware/fink-client). Either try the tutorial, or contact us so that we can deploy it for you!

## Beyond known moving objects: new discoveries

So far, we only mentioned moving objects that are already contained in the MPC database. But there are potentially more objects still uncatalogued. In Fink, we have a science module to try to catch this potential Solar System new candidates. It works in 2 steps:

### Step 1: filtering candidates

Similarly to MPC, we try to guess in real-time if an alert could be a moving object. This time, we do not have any name information, so we have to guess with other properties. Here is the summary of our cuts:

1. No stellar counterpart from PS1, `sgscore1` < 0.76 (Tachibana & Miller 2018)
2. Number of detection is 1 or 2
3. No Panstarrs counterpart within 1"
4. If 2 detections, observations must be done within 30 min.

There are about 100 times less candidates than confirmed SSO, with about 100-1000 candidates per night:

![sso-cand-evolution](/images/sso-cand-evolution.png)
_Nightly number of alerts flagged as Solar System objects candidates, taken from https://fink-portal.org/stats. After 2 years of operations (Nov 2019 to December 2021, 523 observing nights), Fink has flagged more than 300,000 alerts as SSO candidates._

### Step 2: refining

All candidates are probably not SSO, so the second step is to refine the analysis by linking those candidates and extracting orbits. This is a work in progress -- more to come soon!