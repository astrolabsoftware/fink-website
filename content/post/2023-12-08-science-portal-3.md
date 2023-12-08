---
layout: post
date: 2023-12-07
title: Science Portal 3.0
author: fink
tags: [award]
---

<img src="/images/science-portal-3/frontpage.png" width="100%" height="100%" style="display: block; margin: auto;" />

The Fink Science Portal 3.0 is out! This new version contains a lot of improvements and new features, with a huge contribution from Sergey Karpov (FZU). The full list of code change can be found in the [release notes](https://github.com/astrolabsoftware/fink-science-portal/releases/tag/3.0), and we detail below the main changes.

### New features

This new release brings a lot of new features. Regarding the general layout, we added a responsive menubar on top to complement the current togglable sidebar. We also implement the auto-close of the sidebar when clicking on a link.

Then the object page benefited from most of the updates. The lightcurve helper is now moved in a popover below the lightcurve. User can also toggle the visibility of lightcurve upper limits independently of the rest, and we display the information on whether a lightcurve point is negative or low quality in the popup of a measurement.

On the object side accordion, the content of the `Alert content` dropdown is updated when clicking on a measurement in the lightcurve (in addition to the change of cutouts) so that user can explore all alerts information more easily.

Last but no least, the direct download of object data has been enabled with buttons (JSON & VOTable)!

### Layout improvement or change

The layout of the portal in general has been improved keep the same content on all platforms, including mobile devices. This was done by enforcing better use of CSS.

In the main page, the result of a query is now presented in a scrollable table, instead of a multi-page table. This will improve the experience on touch screen such as tablets or smartphones.

Regarding elements of the portal, the main change concerns the display and interaction with the cutouts. Cutouts are bigger, uncropped, and zooming in one will automatically update the zoom in the others. The default color map has been set to `Blues`. The `Alert content` accordion contains a scrollable table instead of a multi-page table.

Note finally that irrelevant tabs in the object page are automatically disabled (e.g. disable Solar System when there is no match with MPC), and the external link URL display has been improved.

### Code improvement and bug fix

- Generate QR code asynchronously to speed-up the page loading
- Remove HBase access outside the API part
- Enable easier Dash callback debugging

### API change

- `lc_features_g` and `lc_features_r` have been casted into string arrays.
- We added a new argument in `/api/v1/ssocand` to control the maximum number of entries to return (`maxnumber`, default is 10,000).
- The documentation for resolvers have been added.