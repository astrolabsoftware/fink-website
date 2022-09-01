---
layout: post
date: 2022-08-30
title: Fink Science Portal 2.0
author: fink
tags: [Portal]
---

Check out the details of the version 2.0 of the Science Portal. A shiny new interface, better performance, and some new features or bug fixes ([release notes](https://github.com/astrolabsoftware/fink-science-portal/releases/tag/2.0)). Do not hesitate to give your feedback, and propose missing features!

<!--more-->

![1](/images/main_v2.png)

The main page did not change, and you will find the usual query buttons. The navbar changed a bit, with buttons on the top right to access various pages.

![2](/images/query_results_v2.png)

After you make a query, you will access the results via 3 tabs: info (general information on the data displayed), Table (list of matching results), and Sky map (matching results projected on the sky). You can customise the table results by opening the `Table options` dropdown (closed by default). You can also preview the first 10 results by pressing the `Preview button`.

### Object page

![3](/images/object_summary_v2.png)

#### Left panel

Once you click on an object link, you will open the page for this object. On the left, you have a card summarising important properties for this object, as well as the set of unique classifications for all alerts associated to this object (e.g. this object is made of alerts with `Unknown`, `SN candidate`, and `Early SN Ia candidate` classifications). Below, you have access to external catalogs and images from the CDS Strasbourg via the Aladin Lite application.

#### Middle panel

In the middle, you will find the lightcurve of the object, that is the data from all alerts that were processed by Fink. We receive all public alerts from ZTF, but only ~70% satisfy our quality cuts, therefore we use different markers to specify the type of data:
1. Circles with error bars show valid alerts that pass the Fink quality cuts.
2. upper triangles with errors, represent alert measurements that do not satisfy Fink quality cuts, but are nevetheless contained in the history of valid alerts and used by classifiers.
3. lower triangles, represent 5-sigma mag limit in difference image based on PSF-fit photometry contained in the history of valid alerts.

You can click on circles in the lightcurve to update the cutouts, and metadata on the right. You can also change units in `DC magnitude` (magnitude corrected from the background source), or `DC flux`. Note that only valid alerts (circles) are converted.

#### Right panel

The right panel contains a number of dropdown items. The first one contains the cutouts. They are small, but you can enlarge them by clicking on the button below, and change the date. Other dropdowns contain: a list with all alert properties (`Last alert content`), the code to download the data (`Download data`), information about closest match in other catalogs (`Neighbourhood`), and a QR code to share the data (`Share`).

#### Topical tabs

You can also inspect further the object by clicking on the tabs above: `Supernovae`, `Variable stars`, `Microlensing`, `Solar System`, and `Tracklets`. You will find more information, and tools to dig into the data.
