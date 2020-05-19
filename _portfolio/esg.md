---
layout: single2
title: "ESG - Visualisation"
excerpt: "Simple Scenarios Overview"
classes: wide2
toc: true
toc_sticky: true
toc_label: "Content"
header:
  #overlay_color: "#333"
  image: /assets/images/esg/esg2.png
  teaser: /assets/images/esg.png

---

**Note:**
The full kernel can be accessed in the GitHub repository [esg](https://github.com/pascal-winter/esg/){: .btn .btn--success}
- Main code: [esg_main.py](https://github.com/pascal-winter/esg/blob/master/esg_main.py)
- Result analysis script: [esg_results.py](https://github.com/pascal-winter/esg/blob/master/scripts/esg_results.py)
- Library: [esglib.py](https://github.com/pascal-winter/esg/blob/master/libpw/esglib.py)
{: .notice--warning}


From an insurance practitioner standpoint, and beyond the technicality of the models used - an important part of using Stochastic simulation is the ability to grasp and understand the impacts of financial fluctuations on the underlying portfolio studied. To this perspective - the **visualisation and characterisation** of the scenarios used are key to assess their appropriateness in a given context.

This routine provides a graphical and numerical overview of a scenario set in order to validate or reject the set for further tests.
For illustration purposes - it is applied here to a plain vanilla Risk Neutral simulation of 3 stocks using a classic lognormal approach.


## Expected Returns Visualisation

Depending on the purpose, one can select either "Risk Neutral" or "Real World" simulation.
In the first case, the forward curve will be used as the expected return for all asset classes, while on the second, their return will generally be an assumption derived from a Risk / Return approach.

Below is the visualisation of the mean returns of 3 simulated assets in a Risk Neutral environment - for 1000 scenarios - along with the initial forward curve passed as parameters *(note: in this case mid and high vol assets pay cash dividends, hence the lower returns)*.

![Test](/assets/images/esg/Esg_Returns.png){: .align-center}

From such visualisation, one can make two observations:
- the mean returns roughly follows the expected returns
- for the higher volatility assets, the noise could be significant

**Take-away:**
- The scenarios are consistent with the base return hypothesis
- If practical - once could elect to increase the number of scenarios to reduce noise on high vol assets
{: .notice--info}

## Percentile Analysis and Volatility

The percentile levels shown below are 99.5%, 99%, 95%, 90%, 75%, 50% and their symmetric.

### Graphical View

![Test](/assets/images/esg/Esg_Percentile.png){: .align-center}

**Take-away:**
- The volatility simulated is in line with the underlying assumptions
- Noise is present on extreme scenarios
{: .notice--info}

### Dashboard

An illustrative Dashboard is here [RunAnalysis.xlsx](https://github.com/pascal-winter/esg/blob/master/_DATA/run1_analysis.xlsx){: .btn .btn--success}

Typically, once the mean returns and volatility are validated, one will look at the percentiles at given time-span to get a grasp of the severity of assumed shocks.

## Other Tests
### The "Martingale Test"
<div>
 <p align="center">
   <img src="/assets/images/wip2.png" alt="wip"
 	   title="Under Construction" width="250" height="100" />
 </p>
</div>
### Correlation Assessment
<div>
 <p align="center">
   <img src="/assets/images/wip2.png" alt="wip"
 	   title="Under Construction" width="250" height="100" />
 </p>
</div>


## Discussion

"All models are wrong" - and beyond technical accuracy and precision is the ability to extract insights. To this perspective, a detailed understanding of assumptions used is extremely valuable.
Being able to validate underlying assumptions on large scenario dataset and get a grasp of return percentiles at different maturities is key to familiarize oneself with the stochastic dataset and avoid opacity on a key assumption of the model.

**Resources:**
- [ESG - A Practical Guide](https://www.soa.org/globalassets/assets/Files/Research/Projects/research-2016-economic-scenario-generators.pdf)) *(SOA, 2016)*
- [ESG Risk Neutral - Calibration and Validation Test](http://www.ressources-actuarielles.net/EXT/ISFA/1226-02.nsf/0/8b04ce005a780ef1c125822900462726/$FILE/BOLLOTTE_Florian_11412509_M%C3%A9moire.pdf)) *(Florian BOLOTTE, 2016)*
{: .notice--warning}
