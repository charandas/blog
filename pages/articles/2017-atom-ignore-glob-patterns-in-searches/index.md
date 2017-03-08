---
title: "How to ignore certain patterns in Atom Search"
date: "2017-03-08T22:09:15Z"
layout: post
path: "/atom-ignore-pattern/"
category: "Atom"
description: "Getting Atom search to ignore specific folders based on glob patterns."
---

Here are two ways for getting Atom to ignore specific folders such as `npm_modules`:

1. "Ignored Names" in "Core Settings"
2. By comma-separated bang-prefixed names in search itself: so something like "humanflow/blog, !public, !node_modules"

<figure>
	<img class="floatCenter" style="max-width: 868px; height: 690.5px;" src="./Atom-ignore-patterns.png" alt="Atom Settings">
	<figcaption>Optimizing Search through Globpatterns</figcaption>
</figure>
