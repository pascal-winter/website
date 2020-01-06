
## This file compiles the MD cheat sheet
Lots of stuff to be found in the folder `/minmist_exemples/docs/-docs`


## Indentation etc...

This is an *italic* character  
This is a **bold** character  
This is an ***itabold*** character  
This is a `code or path `

This is a bullet point list:
* Point
* Point 2

This is a comment box
> Piece or box that appears   
> And stays

This is a line:

---


This is a numbered list:
1. first
2. second   
   second


## Links and Pics
##### Include image based on site base URL:  
![Test]({{site.baseurl}}/assets/images/wip_small.jpg)
##### Attach GIF:
<figure>
  <img src="{{ '/assets/images/mm-bundle-install.gif' | relative_url }}" alt="bundle install in Terminal window">
</figure>

##### Include resized image with reference to basurl:
<div>
 <p align="left">
   <img src="{{site.baseurl}}/assets/images/wip_small.jpg" alt="wip"
 	   title="Under Construction" width="150" height="100" />
 </p>
</div>

##### Link
This is a link: [Tontine Pensions](https://scholarship.law.upenn.edu/penn_law_review/vol163/iss3/3/)   
This is a link to baseurl: [*Quick-Start Guide*]({{ "/docs/quick-start-guide/" | relative_url }})

## Code

```yaml
defaults:
  - scope:
      path: ""
      type: posts
      layout: single
      read_time: true
```


```python
import pandas as pd
import seaborn as sns

from pathlib import Path
CWD = Path.cwd()
# Set the color palette
sns.set_palette(sns.color_palette("Paired"))

```


## Specific to minimal mistake theme

### ProTip box

**ProTip:** Text
{: .notice--info}

### Note box
**Note:** Text
{: .notice--warning}

### Download box link
This is a download link:[<i class="fas fa-download"></i> Download Minimal Mistakes Theme](https://github.com/mmistakes/minimal-mistakes/archive/master.zip){: .btn .btn--success}

### Danger link
**v4 Breaking Change:** Text
{: .notice--danger}
