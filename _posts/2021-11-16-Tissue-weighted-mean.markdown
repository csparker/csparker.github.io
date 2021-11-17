---
layout: post
title:  "Tissue-weighted means"
date:   2021-11-16 12:14:23 +0000
categories: research
---

This post describes the tissue-weighted mean, a new way of summarising NODDI tissue microstructure metrics in regions-of-interest.
Read the official publication on the tissue-weighted mean [here][tissue-weighted-mean-preprint].


## Why tissue-weighted means?

# Conventional means are biased.

The goal of calculating an ROI mean of a tissue metric is normally to find its average value across the tissue within the ROI. The conventional way to do this is prone to bias, as shown in the example ROI below.

<br/>
<img src="{{ site.url }}/fig1.png">
<br/>

Figure 1: Conventional mean does not equal the ground truth mean in an ROI containing voxels contaminated by cerebrospinal fluid.

The conventional method computes the mean by summing the tissue metric across all voxels and dividing by the total number of voxels. However, by assuming all voxels contain an equal amount of tissue, the values of metrics in voxels containing less tissue (i.e. more CSF, left-most voxel) are over-weighted, generating a bias. 

This is the case not only when estimating means of NODDI tissue metrics, but also the mean of any other compartmental metric that is derived from a multi-compartment model (such as those from free water elimiation).

# Tissue-weighted means are unbiased.

NODDI estimates the amount of tissue in each voxel, allowing this mis-estimation to be prevented. 

By using NODDI's tissue fraction parameter (1 - FISO), which is enabled by NODDI's explicit representation of the CSF compartment, the tissue-weighted mean instead calculates a weighted mean. The weighted mean sums the metric across the tissue within the ROI and divides by the total tissue volume:

<br/>
![\Large x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}](https://latex.codecogs.com/svg.latex?\Large&space;\mathrm{Tissue{\text -}weighted \, mean=\frac{\sum_{i=1}^{n} TF_{i} M_{i}}{\sum_{i=1}^{n} TF_{i}}})
<br/>

This estimate is unbiased and more accurate in ROIs containing voxels contaminated by CSF, as demonstrated below.

<br/>
<img src="{{ site.url }}/fig2.png">
<br/>

Figure 2: Tissue-weighted mean equals the ground truth mean in an ROI containing voxels contaminated by CSF.

[comment]: <> (<img src="https://render.githubusercontent.com/render/math?math=\frac{1}{2}">)

This prevention of CSF contamination-induced bias is important in studies of ROIs that border CSF and in studies of neurodegenerative disease, where atrophy can lead to larger ventricle volumes and more CSF contamination.

<br/>

## Evidence of bias in real-world data

# Bias is high in patients and in ROIs that border CSF.

The magnitude of bias is significantly different from zero for many ROIs, in both healthy control subjects or patients with YOAD. This is demonstrated by the high number of ROIs with points above the bar in the plot below, denoting that the mean of the bias (the height of the bar) across subjects is not equal to zero.

<br/>
<img src="{{ site.url }}/fig3.png">
<br/>

Figure 3: Bias in healthy subjects and those with young Onset Alzheimers disease (YOAD), for NODDI's tissue microstructure parameters of neurite density index (NDI) and orientation dispersion index (ODI). Positive bias indicates the conventional mean is over-estimated compared to the tissue-weighted mean.

The figure shows that bias is more extreme in the ROIs that border CSF (such as the fornix (FX)) compared to those that do not (such as the superior longitudinal fasciculus (SLF)). Furthermore, YOAD patients (turquoise bars) tend to have more extreme bias than control subjects (red bars), which is due to their larger ventricle volumes, a property arising from brain atrophy.

# Conventional means mis-estimate group differences.


Because the patient group tends to have higher bias than the control group, the tissue-weighted mean tends to correct the ROI mean of the patient group moreso than the control group. This results in a mis-estimation of group differences when using the conventional mean.

<br/>
<img src="{{ site.url }}/fig4.png">
<br/>

Figure 4: Effect sizes for group differences between control and patient groups using the conventional mean (blue) and tissue-weighted mean (red).

This mis-estimation of group differences particularly affects those ROIs that typically border CSF (e.g. FX) or become more contaminated by CSF following brain atrophy (e.g. the hippocampal cingulum (CGH)).

# Bias remains at higher image resolution.

Despite ROIs in higher resolution images containing relatively fewer voxels that border CSF, significant levels of bias are still observed for many ROIs. This is shown in data from the Alzheimer's Disease Neuroimaging Initiative, where voxels occupy half the volume as for the YOAD dataset.

<br/>
<img src="{{ site.url }}/fig5.png">
<br/>
Figure 5: Bias in the conventional means of ADNI data.

As before, bias is higher for ROIs that border CSF. As suggested by the YOAD study, such bias is likely to result in mis-estimation of group differences, due to the greater correction of CSF contamination in the patient group.

<br/>

## Summary

The conventional way to compute ROI means of NODDI tissue metrics is biased when the ROI contains voxels contaminated with CSF (Fig 1, 3). The tissue-weighted mean leverages NODDI's tissue fraction parameter to overcome this (Fig. 2). This allows more accurate estimates of ROI means in the regions bordering CSF and in studies of aging and neurodegenerative disease (Fig. 4). To read more about the tissue-weighted mean and learn how to apply it to your dataset, check out the dedicated [walkthrough][tissue-weighted-mean-walkthrough] or the official [publication][tissue-weighted-mean-preprint].

## References

Parker, C.S., Veale, T., Bocchetta, M., Slattery, C.F., Malone, I.B., Thomas, D.L., Schott, J.M., Cash, D.M. and Zhang, H., 2021. Not all voxels are created equal: reducing estimation bias in regional NODDI metrics using tissue-weighted means. Pre-print in bioRxiv. doi: https://doi.org/10.1101/2021.06.29.450089.


[tissue-weighted-mean-preprint]: https://www.biorxiv.org/content/10.1101/2021.06.29.450089v3.abstract
[tissue-weighted-mean-walkthrough]: https://github.com/tdveale/TissueWeightedMean

