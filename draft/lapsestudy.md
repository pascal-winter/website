---
layout: single2
title: "Lapse Study"
excerpt: "Kaplan Meier for the masses"
classes: wide2
toc: true
toc_sticky: true
toc_label: "Content"
header:
  image: /assets/images/kmlong.png
  teaser: /assets/images/kmsquare.png #equation3.png

---



The documentation for this routine is WIP:
<div>
 <p align="center">
   <img src="/assets/images/wip2.png" alt="wip"
 	   title="Under Construction" width="250" height="100" />
 </p>
</div>



## Methodology

The **Kaplan-Meier estimator** - is the reference for univariate survival function estimation. More information can be found on the [wiki page](https://en.wikipedia.org/wiki/Kaplan%E2%80%93Meier_estimator) or in any statistical resource.

Here is the estimate:   

$$\hat{S}(t) = \prod_{t_i \lt t} \frac{n_i - d_i}{n_i}$$   

with $$d_i$$ the number of "failures" at time $$t$$ and $$n_i$$ the exposed population just before time $$t$$.

**Tip:**   
The **Kaplan-Meier estimator** intuition is generally easy to grasp on a simple practical example. One of them can be found here: [**KaplanMeier_Refresh.xlsx**]({{ "/assets/other/KaplanMeier_Refresh.xlsx" | relative_url }})
{: .notice--warning}



## Data Preparation

<div>
 <p align="center">
   <img src="/assets/images/wip_small.jpg" alt="wip"
 	   title="Under Construction" width="150" height="100" />
 </p>
</div>


## Analysis

### Aggregation & Subset selection



### Kaplan-Meier estimation
https://lifelines.readthedocs.io/en/latest/


### Visualisation
