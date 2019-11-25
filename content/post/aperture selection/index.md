---
title: 'Aperture Selection Methods'
subtitle: 'and how they affect the light curves'
# summary: Create a beautifully simple website in under 10 minutes.
authors:
- admin
# tags:
# - Academic
# categories:
# - Demo
# - Demo
date: 2019-10-31
lastmod: 2019-11-25
featured: false
draft: false
math: true

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Placement options: 1 = Full column width, 2 = Out-set, 3 = Screen-width
# Focal point options: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight
# image:
#   placement: 2
#    caption: 'Image credit: [**Unsplash**](https://unsplash.com/photos/CpkOjOcXdUY)'
#   focal_point: ""
#   preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
# projects: []
---

## Aperture Selection Methods

1. Pipeline method
	- This method uses the aperture that I handpicked for magnitude groups during the summer.
2. Percentile method
	- This method was described in the previous post. It basically identifies sources when they are above ~80th percentile.
3. Threshold method
	- This is a `lightkurve` built-in method that basically chooses the closest source-like feature to the center as our target, as long as it is above a number sigmas above the MAD.

I want to describe the methods a bit more in depth. But not for now. I will update it later. For now I'll settle with showing the results and discussing them. The figures below are two targets where there are drastic changes on the light curves due to aperture selection methods.

{{< figure src=144759493_M12_LCs.jpg title=" TIC=144759493. " >}}

{{< figure src=93912428_M14_LCs.jpg title=" TIC=93912428. " >}}