---
layout: archive
title: "IPA - Formalisation"
permalink: /ipa/formalisation/
sidebar:
  nav: "docs"
classes: wide
---

## Fund Concept  <a name="concept"></a>


### IPA Overview

The Individual Pooled Annuity can be seen as a generalization of a
classic Tontine -- with an heterogenous population, an open subscription
mechanism and flexible outgoes (fixed). Once subscribed, there is no
withdrawal and proceedings are to be paid at the maturities selected at
onboarding upon survival. As a standard annuity or tontine, there are no
benefits paid upon death, and the proceedings of deceased members are to
be allocated among the survival peers.

### Fund Allocation

<div>
 <p align="center">
   <img src="{{site.baseurl}}/assets/images/wip_small.jpg" alt="wip"
 	   title="Under Construction" width="150" height="100" />
 </p>
</div>

### IPA Mechanism

<div>
 <p align="center">
   <img src="{{site.baseurl}}/assets/images/wip_small.jpg" alt="wip"
 	   title="Under Construction" width="150" height="100" />
 </p>
</div>

### IPA Actuarial Fairness

<div>
 <p align="center">
   <img src="{{site.baseurl}}/assets/images/wip_small.jpg" alt="wip"
 	   title="Under Construction" width="150" height="100" />
 </p>
</div>

### IPF Alternative

From a commercial perspective, offering only non-redeemable option
without any benefit in case death is a key limiting factor. Though not
explored in this article, it is to be noted that it is technically
possible to bundle an "IPF" -- Individual Pooled Fund -- where the
members retain the flexibility of withdrawals along with the balance
returned to beneficiary in case of death. The IPF could use the same
fund management infrastructure --but, from actuarial fairness
perspective, this IPF could not benefit from the IPA additional
longevity returns.

### Value proposition - Summary

<table>
<thead>
<tr class="header">
<th></th>
<th><strong>Pros</strong></th>
<th><strong>Cons</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Customer</td>
<td><p>Additional Gain thanks to Tontine Returns</p>
<p>Lower charge – no hidden annuity fees</p>
<p>Flexibility at issue and for new payments</p>
<p>Transparency of mechanism</p></td>
<td><p>No Benefits upon death</p>
<p>Returns volatility linked to longevity</p>
<p>No redemption possible</p>
<p>Complexity of mechanism</p></td>
</tr>
<tr class="even">
<td>Insurer / Adminsitrator</td>
<td><p>Longevity risk fully hedged in the pool</p>
<p>Market risk transferred to the pool (assuming no guaranteed funds)</p></td>
<td><p>Lower margins due to lower fees</p>
<p>Regulatory framework</p>
<p>Is it still insurance?</p></td>
</tr>
</tbody>
</table>

### Features Comparison – IPA vs IPF

|                                  | IPF: Individual Pooled Fund | IPA: Individual Pooled Annuity |
|----------------------------------|-----------------------------|--------------------------------|
| Fund Selection                   | Unrestricted                | Unrestricted                   |
| Entry Age / Annuity Age          | 0 \~ 110                    | 0 \~ 110                       |
| Change Scheduled Payments        | Yes                         | No                             |
| Change Fund Selection            | Yes                         | Yes                            |
| Contribution Scheme Modification | Yes                         | Yes                            |
| Partial or Full Surrender        | Yes                         | No                             |
| Capital on Death                 | Yes                         | No                             |
| Additional Survival Returns      | No                          | Yes                            |

### Exemple

<div>
 <p align="center">
   <img src="{{site.baseurl}}/assets/images/wip_small.jpg" alt="wip"
 	   title="Under Construction" width="150" height="100" />
 </p>
</div>

### Expected Survival Gain

The cornerstone of the model is the **allocation key** to assign mortality
gains to the survival population at each time step. This key, named in
this paper the **"IPA Share"** is based on the prospective mortality
probabilities of each members, weighed by the projected account value
based on the pre-planned annuity payments. This value is referred below
as the "Expected Survival Gain".

In order to approach "Actuarial Fairness" among the members with
different annuity outgoes plans, we found it was important to measure
the expected gains on the whole horizon and break them down at a
time-step level, to ensure pooling at the maximum common period of the
related annuity outgoes.

### Pooling in an actuarially fair manner

-   Age / Gender and other characteristics are supposedly embedded in
    the mortality table assumptions
-   Different horizon and payment terms are embedded in the prospective
    view, the IPA Share defined at a time step level and the allocation
    of mortality gains only up to the maximum common period of the
    considered cash flows
-   The variation of Account Value among members is also reflected in
    the IPA Share calculation. It is to be noted that large outliers
    will impact actuarial fairness
-   The Pool can welcome new entrants at the beginning of every
    recalculation period
-   The personalized asset allocation results in various account value
    evolution. The frequent recalculation of IPA Share allows to reflect
    the impact of volatile results on the IPA Share at each period
    beginning


## Mathematic framework  <a name="mathematics"></a>

<div>
 <p align="center">
   <img src="{{site.baseurl}}/assets/images/wip_small.jpg" alt="wip"
 	   title="Under Construction" width="150" height="100" />
 </p>
</div>
