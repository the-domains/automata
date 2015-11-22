---
inFeed: true
hasPage: true
inNav: false
inLanguage: null
starred: false
keywords: []
description: How we are teaching our Artificial Intelligence to see
datePublished: '2015-11-22T00:27:53.720Z'
dateModified: '2015-11-22T00:27:25.085Z'
title: "The Grid's Eyes"
author: []
sourcePath: _posts/2015-11-21-saliency-extraction.md
published: true
authors: []
publisher:
  name: null
  domain: null
  url: null
  favicon: null
url: saliency-extraction/index.html
_type: Article

---
# The Grid's Eyes

Since last year I'm working together with [Jon Nordby][0], [Henri Bergius][1] and [Gabriela Thumé][2] on an **image analytics pipeline** to [The Grid][3]. Basically, we are teaching our AI to identify patterns and extract relevant information from raw image data.

We apply algorithms and techniques from the Computer Vision field. Combined with hand picked datasets we can create _extractors_ for features we are interested in. With that our AI can answer questions like _"what is a good color palette from this image?"_, _"where is the best place to crop that picture?"_ or _"can we place text on top of this image? and where?"_.

Here I describe some of these feature extractors and how they impact the design of content you share on The Grid.

# Discovering salient regions

When we look into a picture we can naturally determine what points get our attention first. A human face, a specific object or a region with higher contrast, those are both examples of _salient areas_ or _regions_. We can easily recognize those areas, but computers don't. So we have to teach them.

[The Grid][3] uses a [saliency detection algorithm][4] which analyzes a given image distinguishing foreground and background regions. In summary, it divides the original image by means of little segments. Those segments are analyzed and separated in foreground and background groups. Segments similar to a foreground group are grouped together. After many iterations we have all pixels grouped in their respective groups. If we mark each pixel with a value symbolizing its group, we have what we call a saliency map.

It's easier to understand with an example. Given the original image, we have the following saliency map.
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/e21cd628-9bb1-4a14-8c70-93132207dc76.jpg)
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/a2f1da5d-f96b-4942-b2a5-5bc3c2bb7886.png)

Higher gray levels indicate more salient areas. [The Grid][3]'s AI now knows the region in white is the most relevant part of that image.

With this new information, [The Grid][3]'s Design Systems can now crop the original image to show only those regions to fit on any screen size, for example. Or it can place text on top of images avoiding those salient regions.

# Color palette extraction

**Caliper** - how we call The Grid's image analytics pipeline - reduces all image colors into a really small set of _n_ colors. This set represents the most predominant colors of an image. The Grid's[Colorverse][5] can now use this **color palette** as input to find the right contrastant color to paint some content placed on top of the image, or to generate a similar color to paint the background around an image.

# Face detection

We use Machine Learning to detect if an image has human faces on it and where they are. Combined with the information we already have about salient regions, The Grid's Design Systems can avoid cropping heads off or placing content on top of faces.
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/8d64e55c-04a0-4442-9c11-55deeb11dba5.png)

Face detection is also important to detect mood on face expressions and to recognize people.

# Metadata extraction

Images captured by digital cameras or processed by photo editing softwares include metadata like EXIF tags into their files. **Caliper** extracts information like rotation angle and geolocation. It's important to pre-process images, rotating them back to the expected direction, for example.

Other high level information like color histograms are also extracted and are used by The Grid's Design Systems to calculate contrast and other metrics.

# The infrastructure

The counterpart of Caliper is [ImgFlo][6]. While Caliper does image analyses, ImgFlo is responsable to process every image on The Grid. In other words, ImgFlo applies filters to images and does all kind of transformations.

Both Caliper, ImgFlo and all The Grid backend infrastructure is powered by a dataflow implementation also developed by us: [NoFlo][7] and [Flowhub][8]. Using a dataflow paradigm on Caliper makes it really simple to understand the flow of data between image analisys operations. We can actually show what Caliper looks like because it's not code but a graph.
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/0b7459a9-89c3-4d15-b42f-d9a56942898d.png)

It's a common NoFlo graph: each _node_ is a component that implements some operation (like _Detect Faces_ or _Extract Saliency_). Those components are connected by _edges_ where data flow through. In this case we have image data flowing through the graph and each node is extracting the features we discussed before.

# The future

One of the important aspects of our pipeline is scaling. We are currently working on new feature extractors like for _text detection_. That will make it possible to avoid placing text on top of image's text. We are also improving our existing solution for _face detection_ and researching Deep Learning models for _image classification_ and _sentiment analysis_.

Besides image analysis, we are always researching and experimenting alternative ways to use data extracted by **Caliper**. For example, we can use extracted information from text, images or videos to synthesize new content. Using text as a _seed_ we can generate an infinite number of unique images combining different shapes, curves and colors. ![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/14681f04-d1fa-4a17-a11f-0e6d496bc429.gif)
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/cb196b4b-f9ee-4373-b5c0-32eb44cd1d87.gif)
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/6e59ed0e-a452-4734-a309-caca28afb730.gif)
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/eef44bb5-d812-4a7f-be60-d35470f6bc54.gif)
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/f533ae46-ef95-4eaf-933d-12233b3be196.gif)
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/d1b8eeb0-7695-48a0-b763-81bf63eac01b.gif)
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/a971a657-7a64-4c00-b58e-60d8d6be0420.gif)
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/a81126df-afd2-4ca6-bd6e-b1d154b13fb2.gif)

We can apply more complex algorithms like Delaunay Triangulation to obtain meshes for favbanners or backgrounds.
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/0f3d1805-0cf4-464c-ac58-ba0fff138c6d.png)
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/e574c52a-79cb-4234-9c0b-ec3510514846.png)
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/5cb1d296-8e78-4323-b72e-84576a38cdb5.png)
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/b96a941d-0f02-46ca-a6d9-f87f15c568f3.png)
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/685653d7-7521-49be-a01a-d568ab5525d2.png)
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/247e593c-98db-4198-b358-f6b078588047.png)
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/022670b2-06d3-4a94-9f8c-51a56afdcbb3.png)

[0]: http://jonnor.com/
[1]: http://bergie.today/
[2]: http://gabithu.me/
[3]: https://thegrid.io/
[4]: https://github.com/the-grid/gmr-saliency
[5]: https://www.youtube.com/watch?v=5eKzhw-RVD8
[6]: http://imgflo.org/
[7]: http://noflojs.org/
[8]: http://flowhub.io/