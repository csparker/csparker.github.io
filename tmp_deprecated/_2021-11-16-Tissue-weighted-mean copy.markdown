---
layout: post
title:  "Not all voxels are created equal: reducing estimation bias in regional NODDI metrics using tissue-weighted means"
date:   2021-11-16 12:14:23 +0000
categories: research
---

This post describes the tissue-weighted mean, a more accurate way of averaging NODDI tissue microstructure metrics in regions-of-interest. The tissue-weighted mean fixes a hithero unrecognised problem in ROI analysis that occurs after partial volume effects have been corrected ([Zhang _et. al._ 2012, _Neuroimage_](#references)).


## What is the problem?

Even though NODDI obtains tissue-specific metrics by accounting for CSF partial volume ([Metzler-Baddeley _et. al._, 2012, _Neuroimage_](#references)), the conventional mean averages the metric over all voxels indiscriminately, ignoring the variation in tissue volume. This produces biased estimates, as shown in the example ROI below. 

<br/>
<img src="{{ site.url }}/fig1.png">
<br/>
<font size="2"> <strong>Figure 1:</strong> Conventional mean does not equal the ground truth mean in an ROI containing voxels contaminated by cerebrospinal fluid. </font>
<br/>

In the ROI above, metrics from voxels containing CSF (i.e. left-most voxel) are over-weighted when computing the mean using the conventional method. The degree of mis-estimation is quantified by the bias, which is equal to the difference bbetween the conventional mean and the ground truth mean of the tissue.

This is the case not only when estimating means of NODDI tissue metrics, but also the mean of any other compartmental metric that is derived from a multi-compartment model, such as those from free water elimiation ([Pasternak _et. al._ 2009, _Mag. Res. Med._](#references)).


## What is the tissue-weighted mean?

NODDI estimates the amount of tissue in each voxel, allowing this mis-estimation to be prevented. 

By using NODDI's tissue fraction parameter (1 - FISO), which is enabled by NODDI's explicit representation of the CSF and tissue compartments, the tissue-weighted mean instead calculates a weighted mean. The weighted mean sums the metric across the tissue within the ROI and divides by the total tissue volume:


<br/>
![\Large](https://latex.codecogs.com/svg.latex?\Large&space;\mathrm{Tissue{\text -}weighted \, mean=\frac{\sum_{i=1}^{n} TF_{i} M_{i}}{\sum_{i=1}^{n} TF_{i}}})
<br/>

This estimate is unbiased and more accurate in ROIs containing voxels contaminated by CSF, as demonstrated below.

<br/>
<img src="{{ site.url }}/fig2.png">
<br/>
<font size="2"> <strong>Figure 2:</strong> Tissue-weighted mean equals the ground truth mean in an ROI containing voxels contaminated by CSF. </font>
<br/>

[comment]: <> (<img src="https://render.githubusercontent.com/render/math?math=\frac{1}{2}">)

This prevention of CSF contamination-induced bias is important in studies of ROIs that border CSF and in studies of neurodegenerative disease, where atrophy can lead to larger ventricle volumes and more CSF contamination.

<br/>

## Evidence of bias in real-world data

# Bias is high in patients and in ROIs that border CSF.

The magnitude of bias is significantly different from zero for many ROIs, in both healthy control subjects or patients with YOAD. This is demonstrated by the high number of ROIs with points above the bar in the plot below, denoting that the mean of the bias (the height of the bar) across subjects is not equal to zero.

<br/>
<img src="{{ site.url }}/fig3.png">
<br/>
<font size="2"> <strong>Figure 3:</strong> Bias in healthy subjects and those with young Onset Alzheimers disease (YOAD), for NODDI's tissue microstructure parameters of neurite density 
index (NDI) and orientation dispersion index (ODI). Positive bias indicates the conventional mean is over-estimated compared to the tissue-weighted mean. </font>
<br/>

The figure shows that bias is more extreme in the ROIs that border CSF (such as the fornix (FX)) compared to those that do not (such as the superior longitudinal fasciculus (SLF)). Furthermore, YOAD patients (turquoise bars) tend to have more extreme bias than control subjects (red bars), which is due to their larger ventricle volumes, a property arising from brain atrophy.

# Conventional means mis-estimate group differences.


Because the patient group tends to have higher bias than the control group, the tissue-weighted mean tends to correct the ROI mean of the patient group moreso than the control group. This results in a mis-estimation of group differences when using the conventional mean.

<br/>
<img src="{{ site.url }}/fig4.png">
<br/>
<font size="2"> <strong>Figure 4:</strong> Effect sizes for group differences between control and patient groups using the conventional mean (blue) and tissue-weighted mean (red). </font>
<br/>

This mis-estimation of group differences particularly affects those ROIs that typically border CSF (e.g. FX) or become more contaminated by CSF following brain atrophy (e.g. the hippocampal cingulum (CGH)).

# Bias remains at higher image resolution.

Despite ROIs in higher resolution images containing relatively fewer voxels that border CSF, significant levels of bias are still observed for many ROIs. This is shown in data from the Alzheimer's Disease Neuroimaging Initiative, where voxels occupy half the volume as for the YOAD dataset.

<br/>
<img src="{{ site.url }}/fig5.png">
<br/>
<font size="2"> <strong>Figure 5:</strong> Bias in the conventional means of ADNI data. </font>
<br/>

As before, bias is higher for ROIs that border CSF. As suggested by the YOAD study, such bias is likely to result in mis-estimation of group differences, due to the greater correction of CSF contamination in the patient group.

<br/>

## Summary

The conventional way to compute ROI means of NODDI tissue metrics is biased when the ROI contains voxels contaminated with CSF (Fig 1, 3). The tissue-weighted mean leverages NODDI's tissue fraction parameter to overcome this (Fig. 2). This allows more accurate estimates of ROI means in the regions bordering CSF and in studies of aging and neurodegenerative disease (Fig. 4). To read more about the tissue-weighted mean and learn how to apply it to your dataset, check out the dedicated [walkthrough][tissue-weighted-mean-walkthrough] or the official [publication][tissue-weighted-mean-preprint].

<br/>

## References

Zhang, H., Schneider, T., Wheeler-Kingshott, C. A. & Alexander, D. C. NODDI: practical in vivo neurite orientation dispersion and density imaging of the human brain. _Neuroimage_ 61, 1000–1016 (2012). 

Metzler-Baddeley, C., O’Sullivan, M. J., Bells, S., Pasternak, O. & Jones, D. K. How and how not to correct for CSF-contamination in diffusion MRI. _Neuroimage_ 59, 1394–1403 (2012). 

Pasternak, O., Sochen, N., Gur, Y., Intrator, N. and Assaf, Y. Free water elimination and mapping from diffusion MRI. _Magnetic Resonance in Medicine: An Official Journal of the International Society for Magnetic Resonance in Medicine_ 62(3), 717-730 (2009).

**Parker, C.S.**, **Veale, T.**, Bocchetta, M., Slattery, C.F., Malone, I.B., Thomas, D.L., Schott, J.M., Cash, D.M. & Zhang, H., 2021. Not all voxels are created equal: reducing estimation bias in regional NODDI metrics using tissue-weighted means. Pre-print in _bioRxiv_. doi: [https://doi.org/10.1101/2021.06.29.450089][tissue-weighted-mean-doi]


[tissue-weighted-mean-preprint]: https://www.biorxiv.org/content/10.1101/2021.06.29.450089v3.abstract
[tissue-weighted-mean-walkthrough]: https://github.com/tdveale/TissueWeightedMean
[tissue-weighted-mean-doi]: https://doi.org/10.1101/2021.06.29.450089

