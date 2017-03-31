# ContinuousTransformations

[![Project Status: WIP - Initial development is in progress, but there has not yet been a stable, usable release suitable for the public.](http://www.repostatus.org/badges/latest/wip.svg)](http://www.repostatus.org/#wip)
[![Build Status](https://travis-ci.org/tpapp/ContinuousTransformations.jl.svg?branch=master)](https://travis-ci.org/tpapp/ContinuousTransformations.jl)
[![Coverage Status](https://coveralls.io/repos/tpapp/ContinuousTransformations.jl/badge.svg?branch=master&service=github)](https://coveralls.io/github/tpapp/ContinuousTransformations.jl?branch=master)
[![codecov.io](http://codecov.io/github/tpapp/ContinuousTransformations.jl/coverage.svg?branch=master)](http://codecov.io/github/tpapp/ContinuousTransformations.jl?branch=master)

Continuous transformations (or more precisely, homeomorphisms) from ℝ (and two-point compactified version) and ℝⁿ to various open (or closed) sets used in statistics and numerical methods, such as intervals, simplexes, ordered vectors.

**Work in progress, API may change without notice.**

## Overview

This package was born because I was tired of coding the same transformations over and over, with occasional bugs, and wanted something well-tested.

Transformations defined by the package can be

1. called as functions,
2. called as functions and provide the Jacobian determinant or its log as the second value,
3. inverted.

Jacobian determinants and their log are useful for domain transformations in MCMC, numerical integration, etc.

In addition, the package includes types to represent intervals, and some basic methods of working with them. The concept of intervals is slightly different from [IntervalSet.jl](https://github.com/JuliaMath/IntervalSets.jl) and [ValidatedNumerics.jl](https://github.com/dpsanders/ValidatedNumerics.jl), and as a result not compatible with either.

Examples:
```julia
using ContinuousTransformations
t = LOGISTIC   # Transform ℝ to (0,1) using the logistic function.
t(0.0)         # 0.5
inv(t)         # LOGIT
t(0, JAC)      # 0.5,0.25
t(0, LOGJAC)   # 0.5,-1.3862943611198906
domain(t)      # -∞..∞
image(t)       # 0.0..1.0
```

## Example for transforming integrals

```julia
using ContinuousTransformations
using Cubature

f, dom = integral_substitution(INVODDSRATIO, x->exp(-x^2), 0..Inf)
hquadrature(f, extrema(dom)...)[1] ≈ √π/2 # true
```

## Planned

Vector- and matrix-variant transformations:

1. Unit simplex and inverse transform (stick breaking),
2. Unit vector,
3. Correlation matrices (Lewandowski, D., Kurowicka, D., and Joe, H. 2009)

Pull requests are appreciated.
