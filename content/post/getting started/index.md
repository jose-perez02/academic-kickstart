---
title: 'Aperture Selection and Background Subtraction'
subtitle: 'For TESS Image Processing'
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

Reducing astronomical time series data takes many forms. 
The complexity of the processing depends on the astronomy field, and the accuracy or precision needed.
Due to large amount of data astronomers collect, we make software to automatically perform such complex techniques.
During this ongoing fall, I have been dedicating time to exploring two data reduction components; aperture selection and background subtraction.

Aperture selection consists of selecting the pixels on my image that enclose the target stars.
There is an important balance that the pipeline I create must take into account; signal to noise ratio, and amplitude signal.
When selecting half the pixels enclosing a source, the signal to noise ratio is low.
As we increase the pixel selection, we have more information about the source's behavior and therefore our signal increases.
However, if we keep increasing the aperture the amplitude of our oscillating signal decreases as the background level takes over.
A technique we apply for aperture selection takes advantage of the fact that our source's signal is much higher that the backgrounds'.

## Percentile Aperture

We can represent our image's count values at a time $t$ as a function of position, $I(x,y)$.
All of the source pixel counts are significantly higher than the background level.
We can chose an arbitrary percentil value to which we consider the star's pixel counts to have. In my case, I am choosing a percentile value of 80. Once we determine the pixel positions with values above 80th percentile we can measure photometry,
$$
F =  \sum\_{x\_i,\,y\_i}^{\mathrm{apt}} I(x\_i, y\_i).
$$
Since we know the aperture of the source, we now know that the rest of the pixels must represent the background level. Assuming there is no nearby star of course.
$$
F_{B} = \sum\_{x\_i,\,y\_i}^{\mathrm{not apt}} I(x\_i, y\_i)
$$

{{< figure src=new_apt.jpg title=" TESS FFI Cutout with Percentile Aperture selection." >}}

## Background Estimation

The importance of measuring an accurate background level lies in the fact that our images are collecting photons from unwanted sources.
The unwanted sources, just like our target stars, have signals that change over time. 
This means that the signal we collect can be more accurately represeented by our source signal, $S(x,y)$ and the background signal $B(x,y)$.
It is probably important to note that $B(x,y)$ is generally a very uniform function at local scales, but more generally our image's intensity can be represented as,
$$
I(x,y) = S (x,y) + B(x,y) \approx S(x,y) + \bar B
$$

A simple method for background estimation is to take the mean of a sample of intensity values of our image, or even better the median. The standard deviation will give us the error on this estimation. For my first trials I assumed that there is no nearby stars, so I used the pixels not included in my source aperture to calculate the mean. In fact, I will keep using this until I decide is worth removing all sources. Once I get $\bar B$, I can isolate the source and calculate its flux,
$$
S(x,y) = I(x,y) - \bar B, \\\\\\
F =  \sum\_{x\_i,\,y\_i}^{\mathrm{apt}} I(x\_i, y\_i) - \bar B.
$$

This method is repeated on every single frame in order to get the source's flux time series, or light curve. The quality of the techniques mentioned above is rated on the signal to noise ratio of the target star's variability. I'll be exploring one more technique and comparing them in the next post.