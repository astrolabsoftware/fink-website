---
layout: post
date: 2021-07-05
title:  Fink Science Portal 0.8
author: fink
tags: [Portal]
---

Check out the version 0.8 of the Science Portal: increased features, boosted conesearch, improved layout, support for mobile platforms, and many more!
<!--more-->

## New API features

The number of API services has increased. You can retrieve single object information, query the database (conesearch, date search), search by object class, retrieve Solar System Object, retrieve cutouts data, cross-match your favourite catalog data with Fink data, and more!

| HTTP Method | URI | Action | Availability |
|-------------|-----|--------|--------------|
| POST/GET | [link](http://134.158.75.151:24000/api/v1/objects)| Retrieve single object data from the Fink database | &#x2611;&#xFE0F; |
| POST/GET | [link](http://134.158.75.151:24000/api/v1/explorer) | Query the Fink alert database | &#x2611;&#xFE0F; |
| POST/GET | [link](http://134.158.75.151:24000/api/v1/latests) | Get latest alerts by class | &#x2611;&#xFE0F; |
| POST/GET | [link](http://134.158.75.151:24000/api/v1/sso) | Get Solar System Object data | &#x2611;&#xFE0F; |
| POST/GET | [link](http://134.158.75.151:24000/api/v1/cutouts) | Retrieve cutout data from the Fink database| &#x2611;&#xFE0F; |
| POST/GET | [link](http://134.158.75.151:24000/api/v1/xmatch) | Cross-match user-defined catalog with Fink alert data| &#x2611;&#xFE0F; |
| GET  | [link](http://134.158.75.151:24000/api/v1/classes)  | Display all Fink derived classification | &#x2611;&#xFE0F; |
| GET  | [link](http://134.158.75.151:24000/api/v1/columns)  | Display all available alert fields and their type | &#x2611;&#xFE0F; |

More information at [http://134.158.75.151:24000/api](http://134.158.75.151:24000/api).

## Boosted conesearch

The way to perform a conesearch in Fink has changed in two ways:
- You can specify time boundaries in addition to spatial constraints
- Under the hood, we use a multiresolution search to speed-up the queries, allowing users to define search radius as big as 5 degrees!

Here is the current performance of the service for querying a
single object (1.3TB, about 40 million alerts):

![conesearch](https://user-images.githubusercontent.com/20426972/123047697-e493a500-d3fd-11eb-9f30-216dce9cbf43.png)

_circle marks with dashed lines are results for a full scan search
(~2 years of data, 40 million alerts), while the upper triangles with
dotted lines are results when restraining to 7 days search.
The numbers close to markers show the number of objects returned by the conesearch._

We reached several of orders of magnitude speed-up... one can see how bad the first implementation was :D For arcsecond (blue) or arcminute (orange) scale radius, restricting the dates or performing full scan search give similar performances. However, for degree scale radius, it is recommended to specify a time window to speed-up the search.

If you are using the API, the new syntax is now:

```python
r = requests.post(
   'http://134.158.75.151:24000/api/v1/explorer',
   json={
       'ra': ra,
       'dec': dec,
       'radius': radius,
       'startdate_conesearch': startdate, # optional, can be None or not specified
       'window_days_conesearch': window_days # optional, can be None or not specified

   }
)

pdf = pd.read_json(r.content)
```

## Improved layout

We constantly improve the layout to offer you the best experience when data mining Fink data. Among several things, the Home page has been replaced by the Search page: less click to the results! We also improved _views_ (e.g. check out the supernovae one), and introduced new _views_ (e.g. check out the Solar System one). We also did a lot of hugs to the navigation bar, for a better result.

## Support for mobile platforms

I owe you apologies -- I barely use my phone to go on Internet. But when we launched the Twitter account for Fink ([@FinkBroker](https://twitter.com/finkbroker)), you probably almost died of heart attack by seeing the poorly designed Fink Science Portal... This is past now, and you can enjoy Fink on your smartphone! Checkout some beautiful objects:

<img src="/images/mobile1.png" width="100%" height="100%" style="display: block; margin: auto;" />

<img src="/images/mobile2.png" width="100%" height="100%" style="display: block; margin: auto;" />