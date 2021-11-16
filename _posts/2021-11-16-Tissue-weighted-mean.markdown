---
layout: post
title:  "Tissue-weighted means"
date:   2021-11-16 12:14:23 +0000
categories: research
---

This post describes the tissue-weighted mean, a new way of summarising NODDI microstructure metrics in regions-of-interest.
Read the official publication on the tissue-weighted mean [here][tissue-weighted-mean-preprint].

## Why tissue-weighted means?

# The conventional way to compute means of NODDI tissue metrics in regions-of-interest is biased.

Figure 1: Conventional mean differs from ground truth mean in an ROI containing voxels contaminated with cerebrospinal fluid.

The goal of calculating the mean is to average the NODDI tissue metric across the tissue within the ROI. The typical way people calculate this is by summing the metric across all voxels and dividing by the total number of voxels. However, by assuming all voxels have an equal amount of tissue, the metric values in voxels containing less tissue (i.e. more CSF) are over-weighted. This generates bias compared its ground truth. This applies not only when estimating means of NODDI tissue metrics, but also the mean of any compartmental metric derived from a multi-compartment model. Fortunately, measures of the amount of tissue are estimated by NODDI and allow us to prevent mis-estimation.

# Tissue-weighted mean allow unbiased estimation of NODDI tissue metrics in ROIs

Figure 2: Tissue-weighted mean is an unbiased estimate of the ground truth mean

Using the tissue fraction estimates derived from NODDI (1 - FISO parameter), the tissue-weighted mean sums the metric across the tissue within the ROI and divides by the total tissue volume. This produces unbiased estimates of the mean across the tissue. This makes the estimate more accurate in voxels contaminated by CSF. This is important for studies of ROIs that border CSF and in neurodegenerative disease where atrophy can lead to more CSF contamination.

## Evidence of bias in real-world data

# Bias is observed in many ROIs and is worse in patients and ROIs that border CSF

Figure 3: Extent of bias in different white matter ROIs in healthy subjects and patients with young Onset Alzheimers disease (YOAD). Bars shown the difference in mean neurite density index between conventional and tissue-weighted mean. Positive bias indicates the conventional mean is over-estimated.

The magnitude of bias in real world data is significant for many ROIs. Bias is more extreme in YOAD patients and in the ROIs that border CSF, such as the fornix (FX) and corpus callosum (GCC, BCC, SCC). 

# The conventional means can mis-estimate group differences

Figure 4: Effect sizes for group differences between control and patient groups.

The patient group tends to have larger ventricles, due to atrophy, leading to higher CSF contamination and more bias than in the control group. This leads to mis-estimation of group differences when using the conventional mean compared to the tissue-weighted mean.

# Bias remains in higher resolution datasets

Figure 5: Bias in conventional means computed in healthy subjects from ADNI.

Despite ROIs in higher resolution imaging data containing fewer relative voxels with CSF contamination, significant bias is observed, particularly in ROIs that border CSF.

[tissue-weighted-mean-preprint]: https://www.biorxiv.org/content/10.1101/2021.06.29.450089v3.abstract

