---
layout: post
comments: true
title: Presentations of Permutation Groups
---

The work on finding finite presentations of permutation groups was somewhat delayed last week because I've come across a bug in the `reidemester_presentation` code while testing the one step version of the presetation algorithm (which is essentially finished). Turned out it was caused by an unnecessary cyclic reduction on one line in the main elimination technique. See [PR](https://github.com/sympy/sympy/pull/12973) for the explanation.

Another delay was because I realised that for the multi-step version, it would be useful to have the homomorphism [PR](https://github.com/sympy/sympy/pull/12827) merged. Mostly this is because of the `generator_product` method from the `PermutationGroup` class - this takes an arbitrary permutation in the group and returns a list of the strong generators such that the product of the elements of the list is equal to this permutation. But the homomorphism functionality that's already there (the case of the homomorphism from an `FpGroup` to a `PermutationGroup`) would be helpful too. So I switched by attention to getting that PR ready for merging.

So this week I'll try to add the multistep version and will also start implementing homomorphisms from `PermutationGroup`s and to `FpGroup`s, as outlined in the plan. Also, at some point when the rewriting PR is merged, I could try out the potentially improved random element generator for `FpGroup`s that I sketched a couple of weeks ago. This is not officially part of the plan and not in any way urgent, but could probably give better random elements which would be useful for the `order()` method and the kernel calculation for homomorphisms. Though I might not actually get around to it this week.
