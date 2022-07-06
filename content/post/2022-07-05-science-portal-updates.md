---
layout: post
date: 2022-07-05
title: Fink Science Portal 1.3
author: fink
tags: [Portal]
---

Check out the details of the version 1.3 of the Science Portal. This is the last release for the version 1, and the next release will be a major one!
<!--more-->

## SIMBAD taxonomy change

The SIMBAD astronomical database provides basic data, cross-identifications, bibliography and measurements for astronomical objects outside the solar system. Using the [xmatch](https://cdsxmatch.u-strasbg.fr/) service at CDS, we crossmatch in real-time all incoming Fink alerts with the SIMBAD database, and add a new field to alerts `cdsxmatch` with the resulting object label (if found).

Since June 2022, the [historical list](https://simbad.cds.unistra.fr/guide/otypes.old.html) of object types is replaced by this [reorganized list](https://simbad.cds.unistra.fr/guide/otypes.htx), based on physical properties, and on the evolutionary stage for stars. Here is a list of [evolved labels](https://simbad.cds.unistra.fr/guide/otypes.labels.txt).

In Fink, we decided to not apply this replacement backward in time. This means, alerts emitted before the change have their `cdsxmatch` field with the historical taxonomy, while new alerts use the new taxonomy. On the practical side, this means any query based on SIMBAD labels should be made accordingly, e.g. if you are looking for young stellar object candidates, you would look for `Candidate_YSO` before 2022/06, and `YSO_Candidate` after.

NOTE: it appears that only some labels have evolved. For example, new RR Lyrae alerts still come with the old `RRLyr` label. So expect the transition to be spread in time.

## Dealing with the Unknowns

```
+--------------------+--------+------------+
|               class|   count|fraction (%)|
+--------------------+--------+------------+
|             Unknown|20853082|       50.94|
|    Solar System MPC| 5438284|       13.28|
|                  V*| 2762177|        6.75|
|       Candidate_EB*| 2435437|        5.95|
|               RRLyr| 2339206|        5.71|
|                 EB*| 1352870|         3.3|
|       Candidate_LP*|  972726|        2.38|
|                 QSO|  944249|        2.31|
|                Star|  718921|        1.76|
|                Mira|  608670|        1.49|
|     Candidate_RRLyr|  366322|        0.89|
|                LPV*|  296872|        0.73|
|              PulsV*|  209117|        0.51|
|           Seyfert_1|  205695|         0.5|
|        SN candidate|  174625|        0.43|
|Solar System cand...|  154454|        0.38|
|                  C*|   99543|        0.24|
|       Candidate_YSO|   90991|        0.22|
|                 YSO|   68448|        0.17|
|               BLLac|   61754|        0.15|
+--------------------+--------+------------+
only showing top 20 rows
```
_Caption: The table corresponds to the top 20 most represented alert classes among 40,939,007 processed ZTF alerts by Fink in 2021 (12,214,151 unique objects)._

About half of Fink processed alerts does not have a label: no counterpart in SIMBAD, and they were not favoured by any of the classifiers based on available information to us at the time of emission. The second most represented category is `Solar System MPC`, that is known Solar System objects found in the Minor Planet Center database. Then come, excluding candidates, `V*` (generic type), `RRLyr`, `EB*`, `QSO` and `Mira`, that are variable stars found with a crossmatch with the SIMBAD database.

### Where is our blind spot?

![alert_class](/images/alert_classification.png)
_Caption: Number of unique objects as a function of the number of detections per object since the beginning of the ZTF survey, for objects detected in June 2021 (nearly 5 million alerts, or 2 million unique objects). Single detection objects are dominated by known Solar System objects, while long term-variables are generally found beyond 100 detections per object. The plot show all objects in Fink for this month (blue curve), the object with a Fink classification (green curve), and the object without classification (orange curve). Other curves are described in the sections below._

If we look at the distribution of unknown objects as a function of the number of times the corresponding object was seen (orange curve), we see that the main part is made of objects with single detection (left -- new transients, Solar System objects, or just boguses that were not caught by the Fink quality cuts). The next part contains objects with a number of detection lower than ~50. These objects are probably a mix between unknown transients (e.g. different types of supernovae and core-collapse) and variable stars that were observed infrequently. The last part contains objects for which the corresponding object has been detected many times before (> 50). These objects correspond to probably the activity of unknown (uncatalogued) variable stars.

Looking at the classified objects (green curve), Fink is good at classifying brand new alerts (dominated by solar system objects) and very long-term variables — but we need to improve the intermediate region!

One way to reduce the number of objects with no tags in Fink is to increase our crossmatch coverage by using more astronomical catalogs (and more diverse ones). So we decided to increase our focus on variable stars as they are more easily catalogued than transients.

### Gaia DR3

[Gaia DR3](https://www.cosmos.esa.int/web/gaia/dr3) is out for a few weeks now, and we are pleased to announce that Fink now crossmatches systematically in real-time all incoming ZTF alerts with the main catalog using the [xmatch](https://cdsxmatch.u-strasbg.fr/) service at CDS. We extract and attach the parallax information if known, as well as the Gaia DR3 name of the object. This helps disentangling for example between galactic & extra-galactic objects.

To give you the importance of having Gaia information, we started from 40,939,007 alerts processed in 2021. We grouped alerts into objects (12,214,151 unique objects), and crossmatched with Gaia DR3. If we keep only objects with signal-to-noise ratio for the parallax above 5, we obtain 6,147,948 (50%) objects with a counterpart in Gaia DR3! This is quite a lot of known variable stars in the dataset. Among these, a large fraction was not previously tagged in Fink (from crossmatch with the SIMBAD database or classifiers).

![gaia](/images/gaia-slices.png)
_Caption: Positions of Fink/ZTF objects in 3D for those with a constrained parallax in Gaia, for selected slices in distance for visibility. Left to right: 0.4-0.5 kiloparsec (kpc), 1.4-1.5 kpc, 5-6 kpc, 15-20 kpc. Above the kiloparsec, we can clearly see the disk of the Galaxy appearing._

### Catalogs for variable stars: GCVS & VSX

#### GCVS catalog

The General Catalogue of Variable Stars (GCVS) [1] contains data for individual variable objects discovered and named as variable stars by 2020 and located mainly in the Milky Way galaxy. The catalog contains 57,247 stars.

The most represented types after crossmatch are `EA/EW/EB` (Close Binary Eclipsing Systems), `RRAB` and `SRB/SR` (Pulsating Variable Stars). Most labels match the ones from SIMBAD: `Mira -> M`, `EB* -> EA/EB`, ... We also found some discrepancies: `LPV* -> SRB` (long period versus semiregular late-type).

If we look at the distribution of matched objects (see plot above, dashed purple curve), most of the objects found in GCVS are in the intermediate regime (below 1,000 detections per object). The crossmatch information is now available in each alert (`d:gcvs`).

[1] Samus N.N., Kazarovets E.V., Durlevich O.V., Kireeva N.N., Pastukhova E.N.,
General Catalogue of Variable Stars: Version GCVS 5.1,
Astronomy Reports, 2017, vol. 61, No. 1, pp. 80-88 {2017ARep...61...80S}

#### VSX catalog

The International Variable Star Index (VSX) - AAVSO [2] was initially populated with the entire Combined General Catalog of Variable Stars (4.2), and were added published catalogs of red variables from the Northern Sky Variability Survey (NSVS), the detected variables from the 3rd All Sky Automated Survey (ASAS-3), all new variables reported in the various volumes of the Information Bulletin on Variable Stars (IBVS), the Miras and eclipsing binaries found and published from Phase 2 data of the Optical Gravitational Lensing Experiment (OGLE-II), and the bright contact binaries extracted from data of the Robotic Optical Transient Search Experiment (ROTSE-I). Later, the catalog was enriched with the input from the world of registered contributors

We took the version available on 16/07/2021 from Vizier, with objects with non-zero period. This catalog contains 1,933,544 entries. After crossmatch with Fink alert data, the most represented types are `EW/EA` (Close Binary Eclipsing Systems), `SR` (Pulsating Variable Stars). Most labels match those from SIMBAD, but there are new entries as well.

If we look at the distribution of matched objects (see plot above, dashed brown curve), most of the alerts found in VSX are in the intermediate regime (below 1,000 detections per object). The crossmatch information is now available in each alert (`d:vsx`).

[2] Watson C.~L., Henden A.~A., Price A., 2006, SASS, 25, 47

### The Magritte effect

![magritte_effect](/images/magritte_effect.png)
_Caption: Ratio between objects with no labels over the total of objects in the database for June 2021 as a function of the number of detections per object since the beginning of the ZTF survey (the lower the ratio the better). The Fink performance for the previous baseline is shown in blue, while the new performance including Gaia, GCVS and VSX information is shown in orange (version 2.2). Single detection objects are dominated by known Solar System objects, while long term-variable are found in database and catalogs. The addition of external information brings clear benefit as more objects are classified (reaching > 90% classification for objects with around 1,000 detections), although objects with few detections (2-50 range) remains poorly constrained._

If we look now at the total, per objects (top row) or per alerts (bottom row):

<img src="/images/baseline_obj.png" width="35%" height="35%" /><img src="/images/gaia_obj.png" width="40%" height="40%" />

<img src="/images/baseline_alerts.png" width="35%" height="35%" /><img src="/images/gaia_alerts.png" width="40%" height="40%" />

_Labeled objects go from 31% with the previous baseline to 53% with the addition of Gaia, GCVS, and VSX data. Labeled alerts go from 51% to 83%. Gaia, GCVS, and VSX adds mainly variable objects from our galaxy, with many alerts associated._

### Accessing the crossmatch information

The Gaia parallax information, as well as GCVS & VSX crossmatch labels, are available in each alert packet from 2022/07/01 (broker version 2.2, science version 2.0.0). You can easily check it:

![parallax-web](/images/parallax-info.png)

or via the API:

```python
import io
import requests
import pandas as pd

r = requests.post(
  'https://fink-portal.org/api/v1/objects',
  json={
    'objectId': 'ZTF18abtstlx',
    'columns': 'i:candid,i:jd,i:cdsxmatch,d:Plx,d:e_Plx,d:gcvs,d:vsx',
    'output-format': 'json'
  }
)

# Format output in a DataFrame
pdf = pd.read_json(io.BytesIO(r.content))

pdf.head(5)
"""
    d:Plx d:cdsxmatch  d:e_Plx   d:gcvs d:vsx             i:candid          i:jd
0  1.3244         EB*   0.0311  Unknown    EW  2010485411115010129  2.459765e+06
1  1.3244         EB*   0.0311  Unknown    EW  2010483041115010113  2.459765e+06
2  1.3244         EB*   0.0311  Unknown    EW  2010478311115010033  2.459765e+06
3  1.3244         EB*   0.0311  Unknown    EW  2009479251115010014  2.459764e+06
4     NaN         EB*      NaN      nan   nan  1778259431115010015  2.459533e+06
"""
```

Note that alerts processed before the upgrade do not have this information, and fields contain `NaN`.

### Can we do more?

Of course we can, and we should! In Fink, we have a lack of classification for alerts in the intermediate regime (5-500 detections). **Keep in mind: the more we filter out known objects, the better we can focus on potentially exciting new objects!** So let us know if you have a particular catalog for crossmatch, or lightcurve classifier in mind, we would be happy to incorporate it in Fink.

## Grouping moving alerts into objects

Moving objects are special in the sense that each alert has not the same object ID than the others. At the last release (1.2), we introduced the capability to group query results by (static) object ID, and now it is also possible to group by _moving_ object ID:

![grouping](/images/grouping-moving.png)

You can do it for Solar System objects (key `i:ssnamenr`), or man-made objects (key `d:tracklet`).

## Full changelog

Thanks to @fusroman, @BenoitCarry for feedback on the Portal, @AlbertoKroneMartins for feedback on Gaia, and @MariaPruzhinskaya and @SergeyKarpov for information on GCVS and VSX catalogs!

[https://github.com/astrolabsoftware/fink-science-portal/releases/tag/1.3](https://github.com/astrolabsoftware/fink-science-portal/releases/tag/1.3)
