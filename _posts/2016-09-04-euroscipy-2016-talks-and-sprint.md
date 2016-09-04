---
layout: post-no-feature
title: "EuroSciPy 2016 Talks and Sprint"
description: "Second post about EuroSciPy 2016 with summaries of the talks and sprint."
categories: articles
date: 2016-09-04 18:09
---

Continuation of my posts about EuroSciPy 2016. First part can be found
[here][euroscipy_p1].

[euroscipy_p1]({% post_url 2016-09-04-euroscipy-2016-tutorials %})

## Talks part I (day 3)

### Writing code for science
Best practices for writing code for science. Most of the advice is similar
to that of software engineering (e.g. use version control, write functions,
refactor, write tests, etc.). However, writing code when doing science does
differ from conventional software engineering practices. You want to have a fast
feedback loop when doing experiments, so therefore Python and IPython notebooks
are ideal. However, you don't always want to over-engineer your code when
you're just doing experiments and trying things out. The danger in having
everything in a nice module or package is that you may become overly dependent
on existing code and that leads to loss of progressiveness, which is something
we don't want in research. So it's a bit of a tradeoff between having nice
code and doing research efficiently (which may not even be mutually
exclusive...).

### Object detection
On how to use deep learning to do object detection. All I remember was the
last slide where the program was used to detect cat faces.

### Explicit state
State is basically information at a moment in time that your program uses. If
your program depends on state a lot your program will be hard to reason about.
Stateless (e.g. functional) programming can be nice, but not always practical.
Make state and side-effects explicit in your code, e.g. through docstrings or
through a decorator or separating them into functions. That will help manage
state.

### Jupyter and its horizons
In the beginning there was IPython. Then it grew and grew and became very
large. In 2015 it was decided to split up IPython in separate packages (
"The Big Split"). The notebook part was consolidated in the Jupyter project
and is now just called Jupyter notebook. Because it's now not only for Python
anymore, Jupyter notebook also supports a plethora of other languages, such
as Ruby, Julia, R, etc. Some future plans for Jupyter include: new dashboard
view for notebooks, JupyterHub for hosting notebooks.

### Lattice-Boltzmann fluid simulations using walBerla
Lattice-Boltzmann simulations are a type of computational fluid dynamics
methods. They are typically used at the mesoscopic scale, whereas for
example the well-known Navier-Stokes equations are used for macroscopic
simulations, and at the microscopic scale things like particle methods
are used.

### Laplacian operator
Moste PDEs cannot be solved analytically so you need to solve with numerical
methods and discretizing space. The Laplace operator is key in the
Navier-Stokes equations for fluid flows. The discretization used the *octree*
data structure using. For the grids they wrote a Python program called
PABLitO which is a wrapper around another program caled PABLO. For
communcation between meshes MPI using mpi4py was used.

### Untwist
Library for separating audio. In the demo we could hear how drums were removed
from a music sample. Interesting to note: they extended the `numpy.ndarray`
which is apparently not that easy if you want to catch all corner cases (
and they still weren't completely sure they caught everything...).

## Talks part II (day 4)

### Open Science, Lessons from Open Source
Keynote from Abigail from Mozilla who talked about the role of "open" in
software and in science. Making your project more open is not only about open
source, but also about trying to make it as inviting as possible for people to
contribute to your project.

### Glue
Glue seemed like a pretty awesome program. It's an application with a GUI
where you can select multiple data sets, and not only plot them, but also
link data sets together using relationships/connections that exist between
them. What's impressive is that you can select and highlight parts of the data
that you've plotted and the linked (and plotted) data sets will also highlight
accordingly.

### Async with magics
An overview of the various IPython *magics*. These are special commands in
IPython which are used to control it and always start with a `%` symbol.

### Emergency room with Python
The speaker was a PhD student in medicine who who used Pandas to analyze
data from patients from the emergency room. She was able to use it to predict
patient trends.

### Pure data and clean architecture
FP related talk. The talk was about keeping your application core clean and
functional, and isolating your stateful logic to the outher layers, i.e.:
"functional core, imperative shell".

### Vuesz
Vuesz is a graphical scientific plotting package which was made out of
frustration of existing packages that didn't meet the author's needs. It is
implemented using Numpy and PyQt. The plotting itself is custom built for
Vuesz.

### Docker cluster computing
Mostly an introduction to Docker itself and how to use Docker with Kubernetes
to do cluster computing.

### Data Science, Python and the Functional Programming Revolution
Yet another FP related talk. In this talk the usage of FP in data science
was reviewed. The mathematical expressions used in data science are often
elegantly expressed in FP using minimal rewriting of those expressions. This
can also be accomplished in Python by using a more FP style of programming:
for example by using list comprehensions, map, Numpy expressions (e.g.:
numpy.where).

Using the dask library you can also easily parallelize your code. Dask also
seems much easier to write for compared to parallelized Cython or Numexpr.

## Sprint (day 5)
I attended the sprint for `pandas`, the Python data analysis library. Although
I don't work with pandas that much, and when I do it's mostly simple stuff, I
really liked the various data analysis functionalities it provides.

This was the first time I actually participated in a sprint so I didn't have
any high expectations of doing anything remotely useful, nonetheless, I
managed to get a commit into pandas, which is pretty awesome. Granted, it was
just some trivial changes to the documentation, but I did learn quite a bit
from the whole experience. The documentation generation is not that trivial
in larger open source projects like pandas. The documentation is generated
using Sphinx, however some of the code examples are actually executed by
Sphinx using the `..ipython::` directive. So this is a nice way of checking
if the examples in your documentation agree with your actual code.
