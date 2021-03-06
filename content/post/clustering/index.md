---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Clustering in a nutshell"
subtitle: ""
summary: ""
authors: [Gyunam Park]
tags: [data-mining]
categories: [data mining]
date: 2020-04-17T18:32:39+02:00
lastmod: 2020-04-17T18:32:39+02:00
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
*(Last updated: 17. April. 2020)*

This blog post is a supplement for Data Mining instruction at *Business Process Intelligence, RWTH-Aachen*.

### Concept
Clustering is one of the most well-known unsupervised learning methods. There are many existing techniques in the literature. In this post, we will explain the well-known _K-Means Clustering_. This algorithm takes two steps in an iterative manner. 1) We first assign each instance to a cluster by calculating the distance to the randomly given clusters and finding the closest distance. 2) We recompute the clusters by calculating the mean vector of each cluster.

### Clustering Exercise

Let's have a look into the following example.

![IMAGE](resources/CCE3D38ADB4CD58D97DE4966231A10B9.jpg)

The left-hand side is the randomly given initial centroids, and the right-hand side is the instances. Here we want to assign the instances to the closest centroid and recompute centroids based on assigned instances. Here to calculate the distance, we use Euclidian distance: $d(i,j)=\sqrt{(x_{i1}-x_{j1})^2+(x_{i2}=x_{j2})^2+\cdots}$

Let's have a look at the first instance. We calculate the Euclidean distance between this and two centroids and decide which cluster has a closer distance. This has distances of $199,000$ to C1 and $4000$ to C2, so we assign it to C2. Below is the assignment results for others:

![IMAGE](resources/A0DE72DEBC82807265C7B1C377E5DAAB.jpg)

After that, we can recompute the centroids by taking averages of instances belonging to C1 and C2. The new centroids are as follows:

![IMAGE](resources/A6D3BE4E59667168EFBA51052DF31876.jpg)