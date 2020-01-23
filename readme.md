
Thanks to [**Minimal Mistakes**](https://mmistakes.github.io/minimal-mistakes/) for the great theme.

## Life Actuarial Tinkering

### "Bits and Bolts" for practical  use

**Details and walkthrough** can be found here: [**https://pascal-winter.github.io/website/toolkit/**](https://pascal-winter.github.io/website/toolkit/)

#### Current Tools:
* **OPEX study**: allocation example with `PANDAS`
* **ESG**: simple Economic Scenario Generator
* **Lapse Study**: Kaplan Meier for the masses
* **Mortality Study**: Classic portfolio review

#### Other analysis:
* **Titanic**: the infamous Titanic accident dataset from Kaggle


## Rationale

When first introduced to Pandas, Numpy and other scientific packages for "non-IT experts" - it immediately seemed like it had a huge potential for daily actuarial practice:
* Data wrangling capabilities
* Replicability
* Robustness
* Depth of available libraries
* Possible integration in company systems   

This repository regroups some of the routines I developed on my free time while tinkering with these packages.

## Deviation From the Minimal Mistake theme

### Added new option "wide2" for layouts
Added a ``_sass`` folder with copy of `minimal-mistakes.scss` with following code copied at the end:
```html
/* Manually added for a wider layout (right side) */
.wide2 {
  .page {
    float: left;
    width: 100%;
    @include breakpoint($large) {
      padding-left: 0;
    }

    @include breakpoint($x-large) {
      padding-left: 0;
    }
  }

  .page__related {
    @include breakpoint($large) {
      padding-left: 0;
    }

    @include breakpoint($x-large) {
      padding-left: 0;
    }
  }
}
```

### Changed the overall font size
Added the following code in `\assets\css\main.scss`
(after the  code `@import "minimal-mistakes";`)

```html
html {
  font-size: 16px; // change to whatever //16

  @include breakpoint($medium) {
    font-size: 16px; // change to whatever //18
  }

  @include breakpoint($large) {
    font-size: 18px; // change to whatever //20
  }

  @include breakpoint($x-large) {
    font-size: 20px; // change to whatever //22
  }
}
```


### Added MathJax in `_layouts\default.html`

```html
<!--
Added Mathjax
-->

<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  tex2jax: {
    inlineMath: [['$','$'], ['\\(','\\)']],
    processEscapes: true
  }
});
</script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
```
