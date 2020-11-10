
Thanks to [**Minimal Mistakes**](https://mmistakes.github.io/minimal-mistakes/) for the great theme.

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


### Added MathJax in `_includes\scripts.html`
https://www.janmeppe.com/blog/How-to-add-mathjax-to-minimal-mistakes/


```html

<!--
Added Mathjax
-->

<script type="text/javascript" async
	src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/latest.js?config=TeX-MML-AM_CHTML">
</script>

<script type="text/x-mathjax-config">
   MathJax.Hub.Config({
     extensions: ["tex2jax.js"],
     jax: ["input/TeX", "output/HTML-CSS"],
     tex2jax: {
       inlineMath: [ ['$','$'], ["\\(","\\)"] ],
       displayMath: [ ['$$','$$'], ["\\[","\\]"] ],
       processEscapes: true
     },
     "HTML-CSS": { availableFonts: ["TeX"] }
   });
</script>
