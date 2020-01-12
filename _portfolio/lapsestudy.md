---
layout: single2
title: "Lapse Study"
excerpt: "Kaplan Meier for the masses"
classes: wide2
toc: true
#toc_sticky: true
toc_label: "Content"
header:
  image: /assets/images/equation3small.png
  teaser: /assets/images/equation3.png

---

<div>
 <p align="center">
   <img src="{{site.baseurl}}/assets/images/wip_small.jpg" alt="wip"
 	   title="Under Construction" width="150" height="100" />
 </p>
</div>


## Methodology

We will use **Kaplan-Meier estimator** - which is a reference for univariate survival function estimation. More information can be found on the [wiki page](https://en.wikipedia.org/wiki/Kaplan%E2%80%93Meier_estimator) or in any statistical resource.

Here is the estimate:   

$$\hat{S}(t) = \prod_{t_i \lt t} \frac{n_i - d_i}{n_i}$$   

with $$d_i$$ the number of "failures" at time $$t$$ and $$n_i$$ the exposed population just before time $$t$$.


**Tip:**   
If you are new to the **Kaplan-Meier estimator**, a simple practical example is generally the best way to grasp the intuition of the method. An example is available here: [**KaplanMeier_Refresh.xlsx**](https://github.com/pascal-winter/actuarial-tools/tree/master/2.DOCS)
{: .notice--warning}



## Data Preparation

<div>
 <p align="center">
   <img src="{{site.baseurl}}/assets/images/wip_small.jpg" alt="wip"
 	   title="Under Construction" width="150" height="100" />
 </p>
</div>


## Analysis

### Aggregation & Subset selection



### Kaplan-Meier estimation
https://lifelines.readthedocs.io/en/latest/


### Visualisation
