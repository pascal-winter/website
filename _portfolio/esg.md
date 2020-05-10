---
layout: single2
title: "Economic Scenarios Generator"
excerpt: "Visualisation and Characterisation"
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
The full kernel can be accessed in the GitHub repository [bits-and-bolts](https://github.com/pascal-winter/bits-and-bolts/){: .btn .btn--success}
- Main code: [esg_main.py](https://github.com/pascal-winter/bits-and-bolts/blob/master/esg_main.py)
- Result analysis script: [esg_results.py](https://github.com/pascal-winter/bits-and-bolts/blob/master/scripts/esg_results.py)
- Library: [esglib.py](https://github.com/pascal-winter/bits-and-bolts/blob/master/libpw/esglib.py)
{: .notice--warning}


From an insurance practitioner standpoint, and beyond the technicality of the models used - an important part of using Stochastic simulation is the ability to grasp and understand the impacts of fluctuations on the underlying portfolio studied. To this perspective - the **visualisation and characterisation** of the scenarios used are key to assess their appropriateness in a given study context.

Ths routine is a quick way to get an overview of scenario set in order to validate or reject the set for further tests.


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

### Graphical View

![Test](/assets/images/esg/Esg_Percentile.png){: .align-center}

**Take-away:**
- The volatility simulated is in line with the underlying assumptions
- Noise is present on extreme scenarios
{: .notice--info}

### Dashboard

An illustrative Dashboard is here [RunAnalysis.xlsx](https://github.com/pascal-winter/bits-and-bolts/raw/master/_DATA/ESG/run1_analysis.xlsx){: .btn .btn--success}

Typically, once the mean returns and volatility are validated, one will look at the percentiles at given time-span to get a grasp of the severity of assumed shocks.

## Conclusion

"All models are wrong" - but beyond accuracy and precision is the ability to extract insights - and a detailed understanding of assumptions used is extremely valuable to this perspective.
Being able to validate underlying assumptions on large scenario dataset and get a grasp of return percentiles at different maturities is key to familiarize oneself with the stochastic dataset and avoid opacity on a key assumption of the model.
