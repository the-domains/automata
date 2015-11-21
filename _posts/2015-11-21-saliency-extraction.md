---
inFeed: true
hasPage: true
inNav: false
inLanguage: null
starred: false
keywords: []
description: How we are teaching our Artificial Intelligence to see
datePublished: '2015-11-21T21:43:34.478Z'
dateModified: '2015-11-21T21:31:35.351Z'
title: Saliency extraction
author: []
authors: []
publisher:
  name: null
  domain: null
  url: null
  favicon: null
sourcePath: _posts/2015-11-21-saliency-extraction.md
published: true
url: saliency-extraction/index.html
_type: Article

---
Since last year I'm working together with [Jon Nordby][0], [Henri Bergius][1] and [Gabriela Thumé][2] on an image analytics pipeline to [The Grid][3]. Basically, we are teaching our AI to identify patterns and extract relevant information from raw image data.

We apply algorithms and techniques from the Computer Vision field. Combined with hand picked datasets we can create _extractors_ for features we are interested in. With that our AI can answer questions like _"what is a good color palette from this image?"_, _"where is the best place to crop that picture?"_ or _"can we place text on top of this image? and where?"_.

Here I describe some of these feature extractors and how they impact the design of content you share on The Grid.

# Saliency extraction

When we look into a picture we can naturally determine what points get our attention first. A human face, a specific object or a region with higher contrast, those are both examples of _salient areas_ or _regions_. We can easily recognize those areas, but computers don't. So we have to teach them.

[The Grid][3] uses a [saliency detection algorithm][4] which analyzes a given image distinguishing foreground and background regions. In summary, it divides the original image by means of little segments. Those segments are analyzed and separated in foreground and background groups. Segments similar to a foreground group are grouped together. After many iterations we have all pixels grouped in their respective groups. If we mark each pixel with a value symbolizing its group, we have what we call a saliency map.

It's easier to understand with an example. Given the original image, we have the following saliency map.
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/e21cd628-9bb1-4a14-8c70-93132207dc76.jpg)
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/a2f1da5d-f96b-4942-b2a5-5bc3c2bb7886.png)

Higher gray levels indicate more salient areas. [The Grid][3]'s AI now knows the region in white is the most relevant part of that image.

With this new information, [The Grid][3]'s Design Systems can now crop the original image to show only those regions to fit on any screen size, for example. Or it can place text on top of images avoiding those salient regions.

# Color palette extractionFace detectionEXIF extractionThe infrastructure

Image processing (imgflo) and NoFlo/Flowhub.

# The future

Text detection, improve face detection, classification, sentiment analysis (deep learning), iconographer?. New applications (generative images).

Generative images and image processing

Besides analytics, we can use extracted information from text, images or videos to synthesize new content. We can generate an infinite number of unique images using combinations of different shapes, curves and colors.
We can apply more complex algorithms like Delaunay Triangulation to obtain meshes for favbanners or backgrounds.
Generative images by Delaunay Triangulation
Or we can go beyond and use the same algorithm to style existing pictures.
Image styling using Delaunay Triangulation
Using our open-source image processing pipeline imgflo we can manipulate images. It is possible to create any image filter and apply them to enhance pictures.
Filters created on imgflo being applied to the same image. Original picture by Sharon Mollerus.
Imgflo is a runtime for Flowhub, a visual programming environment that made it simple to build image processing graphs.
A Flowhub graph to image processing on imgflo.

[0]: http://jonnor.com/
[1]: http://bergie.today/
[2]: http://gabithu.me/
[3]: https://thegrid.io/
[4]: https://github.com/the-grid/gmr-saliency