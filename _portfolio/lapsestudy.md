---
layout: single2
title: "Persistency Dashboard"
excerpt: "Dynamic Portfolio Visualisation"
classes: wide2
toc: true
toc_sticky: true
toc_label: "Content"
header:
  image: /assets/images/kmlong.png
  teaser: /assets/images/lapse_dash.png #kmsquare.png #equation3.png

---


Lapse, surrender and recovery curves estimation are a key components in the pricing and valuation of Insurance business.    
We look here at how a **dynamic web dashboard** can be used in the day to day actuarial practice.   
<a href="https://app.winter-aas.com/surrenders/" target="_blank">Mockup Dashboard</a>{: .btn .btn--success}


## Benefits of Dynamic dashboards

### Increased need for automation
With increased regulatory and internal segmentation, the periodicity and granularity of assumption update constantly increases - making **automation** of various portfolio subset estimation an important brick in the closing process of insurance companies.    

### Data visualisation
The increasing level of granularity required increases the volume of information to be processed by actuaries, making **data visualisation** a powerful human assistance to detect and manage trends/ abnormalities.

### Flexible, immediate and distributed
The ability to query "on-demand" the estimation on various portfolio subsets provides increased reactivity for **informed decision-making**.


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
