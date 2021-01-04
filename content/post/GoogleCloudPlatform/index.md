---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "Google Cloud Platform"
subtitle: "Getting started with Dataflow"
summary: ""
authors: ["Pieter Coussement"]
tags: []
categories: []
date: 2021-01-04T15:10:50+01:00
lastmod: 2021-01-04T15:10:50+01:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

Last Christmas holiday I was playing around with Google CLoud platform to
improve my "cloud-skills".

The goal was to aggregate and combine/merge different BigQuery datasets using 
Dataflows (Google's implementation of Apache beam).

Using SQL this would have been really easy, as aggregation and joins are simple
one-liners.

Doing this with Apache beam was a bit more tricky.

In the following part I will only discuss topics that for me were pitfalls.

- Get your project ID straight!
- Make sure that your local pipeline works the same way as in the cloud. E.g. 
I had build a pipeline working with 'small' csv, just to get the pipe working. 
My CSV files were read as columns, and thus my whole pipeling worked using 
vectors. When using BigQuery to read the data, the elements are dicts, not rows.
- Keys, keys, keys. This was pretty annoying, as I didn't find  a proper/elegant
answer to it. First element of the element (as in tuple/array) is the key. I had 
to do quite a bit of reshuffling to be able to use the right key (which brings 
me to the next point).
- The merge command is 'beam.CoGroupByKey'. Get your keys together!
- Merges might need to be flattened: 'beam.FlatMap(fn)' with fn a simple iterator to create different elements. Not sure if this is the most elegant way to do this.
- Fix your setup file! and don't forget to run your pipeline using '--setup_file ./setup.py'. This ensure all workernodes get the correct env.
- If you are working with modules add ' packages=find_packages()' to the setup file, so the local modules get loaded in to the workernodes.

What I still need to find out is an easier way to debug. The code now takes about 10 min or so, which is really long in order to be able to debug.

I probably should have chopped it more in the beginning, to see output after every step, and only stitch it together at the end.