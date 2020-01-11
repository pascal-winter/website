---
layout: archive
author_profile: true
permalink: /
title: Life Actuarial Tinkering
classes: wide
header:
  #image: /assets/images/dabi.jpg
  overlay_color: "#333"
  actions:
     - label: "IPA"
       url: "/ipa/"
     - label: "Toolkit"
       url: "/toolkit/"
excerpt: "with a Pythonic approach"
---

## IPA: Individual Pooled Annuities
### A self organised Retirement fund

From an actuarial prospective, a **Tontine** can be seen as a vehicle to pool **longevity risk**.
By generalising its mechanism - we can obtain a fund that provides an interesting solution for retirement and/or investment.

In past few years, several academic studies already explored such ideas:
* [Tontine Pensions](https://scholarship.law.upenn.edu/penn_law_review/vol163/iss3/3/) *(Forman & Sabin, 2015)*
* [Individual Tontine Annuities](https://ssrn.com/abstract=3217551) *(Fullmer, 2018)*
* [Pooled-survival fund](https://www.actuaries.asn.au/Library/Events/FSF/2014/NewfieldPostRetirementPaper140505.pdf) *(Newfield, 2014)*
* [Actuarial fairness and solidarity in pooled annuity funds](https://arxiv.org/abs/1311.5120) *(Donnelly, 2015)*


The **IPA - "Individual Pooled Annuity"** is one of this **Tontine generalisation** - albeit slightly different - since the **"actuarial fairness"** mechanism for allocating mortality proceeds allows:
* Non-homogeneous population (in terms of expected survival rate)
* Open entry mechanism: how to accommodate new entrants at each time steps ?
* Different investment underlyings/allocation for each member
* **Different horizon, maturity and outflow structure for each member**

The detail of the study and proposed allocation can be found in the [**IPA section**]({{ "/ipa" | relative_url }}) of this site.


## Toolkit

### Basic routines for daily actuarial practice

When first introduced to Pandas, Numpy and other scientific packages for "non-IT experts" - it immediately looked to like it had a huge potential for daily actuarial practice:
* Data wrangling capabilities
* Replicability
* Robustness
* Depth of available libraries
* Possible integration in company systems   

In the [**Toolkit section**]({{ "/toolkit" | relative_url }}) of this website, I describe some of the routines I developed on my free time while tinkering with these packages.
