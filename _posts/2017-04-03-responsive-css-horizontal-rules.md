---
layout: post
title: Responsive &ltHR&gt Handling in a Bootstrap Photo Gallery
categories:
- blog
---

I recently completed [Colt Steele's Web Developer Bootcamp](https://www.udemy.com/the-web-developer-bootcamp/learn/v4/), and it caught me up with a lot of web development paradigms that I haven't been paying attention to since my time as a professional Web Developer 6 years ago. Picking it up again is like riding a bike; although, having finished my Computer Science degree, I did notice that the backend sections (with NodeJS and MongoDB) piqued my interests especially. This post deals very much with the front end though.

Idling in the course's [Gitter channel](https://gitter.im/Colt/TheWebDeveloperBootcamp), I occasionally check it to help people who are stuck on one problem or another. Today, an interesting problem of adding horizontal rules stuck out to me as one worth expanding upon.

Given a grid-like photo gallery page displaying three columns at a large viewing resolution, and two columns at a medium resolution, how can the rows be separated with a horizontal rule element? 

Photos in the gallery appear in the source as:

```
<div class="col-lg-4 col-md-6">
  <div class="thumbnail">
    <img [...]
```

Interestingly, including an `<hr>` element after every third `<div>` wrapped image actually doesn't seem to work, and worse, it causes a view disruption when we scale the browser window down to the medium resolution cutoff (≤ 1200px).

The column classes applied in the images should be the first hint; an hr element isn't instructed what column width to span if not specified; so we should specify it:

```
<hr class="col-lg-12">
```

but this now means that after every third image, we have a horizontal rule that forces the next image down one row beneath it; to resolve this, we need to make it so that it can only appear at the large resolution cutoff. [Bootstrap has complementary classes for this](http://getbootstrap.com/css/#responsive-utilities)!

To render a horizontal rule visible at only the large views:

```
<hr class="col-lg-12 visible-lg-block">
```

The equivalent solution for medium views, which must appear every second photo in the gallery:

```
<hr class="col-md-12 visible-md-block">
```

But, this creates duplication at any common multiple of 2 and 3; such as after every 6th photo.

If we were generating the html programmatically (through some JS based view-engine, like EJS), we could just let it duplicate some labour there and it really wouldn't matter; but, since this is some customization to a made-from-scratch gallery, we should really aspire to by [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself). That leads to a case where we need an hr that is both `col-md-12` and `col-lg-12` (in which case, Bootstrap says we just need to state the former), and also `visible-md-block` and `visible-lg-block` (which is the same as the more easily stated: `hidden-xs hidden-s`).

Finally, we have a pattern:

```
...2 photos...
<hr class="style-hr col-md-12 visible-md-block" >
...1 photo...
<hr class="style-hr col-lg-12 visible-lg-block" >
...1 photo...
<hr class="style-hr col-md-12 visible-md-block" >
...2 photos...
<hr class="style-hr col-md-12 hidden-xs hidden-sm">
```

Here is my [codepen](https://codepen.io/mark-walle/full/KWEmdY/) implementation.