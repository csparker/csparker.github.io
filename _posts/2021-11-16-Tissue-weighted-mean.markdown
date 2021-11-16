---
layout: post
title:  "Tissue-weighted means"
date:   2021-11-16 12:14:23 +0000
categories: research
---

This post describes the tissue-weighted mean, a new formula for summarising NODDI microstructure metrics in regions-of-interest.
You can read the official publication on the tissue-weighted mean [here][tissue-weighted-mean-preprint].

## Rationalle

# The conventional way to compute means of NODDI tissue metrics in regions-of-interest is biased.#ß

Figure 1: Conventional mean differs from ground truth mean in an ROI containing voxels contaminated with cerebrospinal fluid.

We would like to know the mean of the NODDI metric across the tissue within the ROI. The typical way people calculate this is by summing the metric across all voxels and dividing by the total number of voxels. However, by assuming all voxels have an equal amount of tissue, the metric values in voxels containing less tissue (i.e. more CSF) are overweighted. This generates a bias in the mean when compared its ground truth. This applies not only when estimating means of NODDI tissue metrics, but the mean of any compartmental metric derived from a multi-compartment model. Luckily, other metrics estimated by NODDI permit a straightforward solution to this mis-estimation problem.


[tissue-weighted-mean-preprint]: https://www.biorxiv.org/content/10.1101/2021.06.29.450089v3.abstract

