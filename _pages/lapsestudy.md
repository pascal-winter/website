---
layout: splash
title: "Surrender Dynamic Estimation"
permalink: /lapse/
author_profile: true
classes: wide # single
#toc: true
#toc_label: "Contents"
#toc_sticky: "true"
header:
  #overlay_color: "#333"
  overlay_image: /assets/images/back2.png
  actions:
     - label: "Dynamic Dashboard"
       url: "https://app.winter-aas.com/surrenders/"
#     - label: "Toolkit"
#       url: "/toolkit/"
excerpt:  Automation and Visualisation
---


Lapse, surrender and recovery curves estimation are a key components in the pricing and valuation of Insurance business.    
**Dynamic web dashboard** can be used in day to day actuarial practice to reduce manual tasks and allow actuaries to focus on **value added** analysis.


## Benefits of Dynamic dashboards
> Regulatory requirements, IFRS17, increased market pressure and internal needs require Actuarial team to increase segmentation, periodicity and granularity of assumption update

### Need for automation
The volume of information to be treated and the number of portfolio subsets to be studied makes **effective automation** an important brick in the closing process of insurance companies.    

### Data visualisation
Complexity and volume of information make **effective data visualisation** a must have to detect and manage trends in portfolios.

### Flexible, immediate and distributed
The ability to query "on-demand" the estimation on various portfolio subsets provides increased reactivity for **informed decision-making** and proactive management.


## Methodology
The **Kaplan-Meier estimator** - is the reference for univariate survival function estimation. More information can be found on the [wiki page](https://en.wikipedia.org/wiki/Kaplan%E2%80%93Meier_estimator) or in any statistical resource.

$$\hat{S}(t) = \prod_{t_i \lt t} \frac{n_i - d_i}{n_i}$$   

with $$d_i$$ the number of "failures" at time $$t$$ and $$n_i$$ the exposed population just before time $$t$$.

**Tip:**   
The **Kaplan-Meier estimator** intuition is generally easy to grasp on a simple practical example. One of them can be found here: [**KaplanMeier_Refresh.xlsx**]({{ "/assets/other/KaplanMeier_Refresh.xlsx" | relative_url }})
{: .notice--warning}

**Lifelines:**   
The lifelines package provides a direct **Kaplan-Meier estimator** within the python environment, along with many other functionalities: [**Lifelines package**](https://lifelines.readthedocs.io/en/latest/)
{: .notice--warning}
