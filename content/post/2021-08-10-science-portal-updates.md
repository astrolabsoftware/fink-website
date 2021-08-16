---
layout: post
date: 2021-08-10
title:  Fink Science Portal 0.9
author: fink
tags: [Portal]
---

Check out the version 0.9 of the Science Portal: Several new features: constellation names, preview of alert information, shiny new buttons, link to SNAD, and... a lot of bug fixes!
<!--more-->

## Constellation

We probably forgot, but the constellations were often used to navigate in the sky. Even though the objects making each constellation have no real relationships between them, the projection they form on the 2D sky is a great tool to locate an object, and target it. This why amateur astronomers use them a lot.

So in Fink, we decided to add the name of the constellation an alert is in. This name will appear in various place in the Science Portal. More information (including screenshots) [here](https://github.com/astrolabsoftware/fink-science-portal/pull/197).

## Preview of alerts

Often you make a query, and you have a long list of candidates. Even though there is table that shows alert properties, it is often more efficient to quickly look at the stamps and the lightcurve to make an educated guess about the relevance of the alert. So instead of manually clicking on individual results, we introduced a new mechanism to quickly inspect alerts: the preview service. If you click on it, a window will open, and show the lightcurves (and some metadata) for the 10 first alerts of your query. Time saved! ([screenshots](https://github.com/astrolabsoftware/fink-science-portal/pull/187))

## Preview of individual alert information

Imagine you open the page for an alert, and you want to see an information contained in the alert packet. Previously, you had to download the alert. Now you can still download the alert, but you can also inspect all alert data in the browser! ([screenshots](https://github.com/astrolabsoftware/fink-science-portal/pull/192)).

## Link to SNAD

The new version of the Portal includes now a direct link to the [SNAD-ZTF](https://ztf.snad.space) viewer, so that you can perform a crossmatch from a Fink alert directly, and inspect possible counterparts in the ZTF-DR4.

## Shiny new buttons

Buttons are important. We click on them everyday, million of times. So we decided to make an effort to have shiny buttons, especially for the links to external catalogs ([screenshots](https://github.com/astrolabsoftware/fink-science-portal/pull/194)).

## Full changelog

[https://github.com/astrolabsoftware/fink-science-portal/milestone/10?closed=1](https://github.com/astrolabsoftware/fink-science-portal/milestone/10?closed=1)
