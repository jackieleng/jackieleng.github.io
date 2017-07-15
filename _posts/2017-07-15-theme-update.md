---
layout: post
title: "Theme Update"
date: 2017-07-15 02:40
categories: update
---

Previously I've been using a theme for this blog that was a bit too fancy
for my tastes, given it being a mere dev blog with very infrequent updates.
Devvy/techy stuff just doesn't seem to work very well with sleeky,
fancy themes is what I've noticed; a more minimalist look would be more
preferable I think.

So after delaying this for months, I've finally sat down to update the
Jekyll theme. I've basically decided to go for the most basic theme possible,
which still looks nice. The `Minima` theme, which is what you get when you
run `jekyll new`, is as barebones as it gets, and fits the bill perfectly.

However, getting Jekyll to run locally was a bit more work. I have no
experience with Ruby at all, which is what is needed to run Jekyll. Installing
Ruby shouldn't be hard I thought, but apparently there are a multitude of
ways to do it. The few I considered were listed on the official Ruby page:
ruby-build, rbenv, RVM, plain apt, but there were many more, which is kind
of surprising to me since I come from a Python background. Finally, I didn't
had the time to consider which was the best method, so I just did
`sudo apt-get install ruby-full`. I ran into some more issues related to
permissions when installing Bundler and Jekyll stuff (likely because I did
not install Ruby using one of the more proper methods), but in the end it
worked out fine.

What's nice about a very minimal theme like this is that it's also easier
for extension and adding your own customisations, which is what I'll likely
be doing in the future.
