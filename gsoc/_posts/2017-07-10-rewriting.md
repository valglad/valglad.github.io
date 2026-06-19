---
layout: post
comments: true
title: The Rewriting System class
---

I sent the [PR](https://github.com/sympy/sympy/pull/12893) with the rewriting system implementation on Wednesday night, i.e. by Thursday as planned. It could have been earlier if I hadn't spent one day experimenting with making the elements of `FpGroup` different from the underlying `FreeGroup`. At the moment they are the same so that, for instance:

```
>>> F, a, b = free_group('a, b')
>>> G = FpGroup(F, [a**2, a*b*a**-1*b**-1])
>>> a in F
True
>>> a in G
True
```

Theoretically, since `F` and `G` are different groups (not even isomorphic), it would make sense for their elements to be represented by different objects, e.g. the generators for `G` could be `a_1, b_1` so that `G` and `F` are connected by the injective homomorphism sending `a_1` and `b_1` to `a` and `b` respectively. Though, for convenience, it could be allowed to refer to the elements of an `FpGroup` by the names of the underlying free group elements (and performing the conversion to `a_1` and `b_1` backstage before passing them onto `FpGroup`'s methods).

Now, if this were done, it would be possible to check for equality of two elements of `FpGroup` using the `==` syntax. E.g., if the generators of `G` were made distinct from `a, b` and called `a_1, b_1`, then we could have `a_1*b_1 == b_1*a_1` (while, of course, `a*b == b*a` is not true because `a` and `b` are the elements of a `FreeGroup`). The latter was the main motivation for trying this out but in the end, after additionally discussing things with my mentor, I left everything as it is and implemented a couple new `FpGroup` methods instead. So, for example, to check if `a*b` and `b*a` are equal in `G`, one would call `G.equals(a*b, b*a)`. The PR has more details.

At the moment I'm in the middle of writing the function for finding finite presentations of permutation groups. I'm slightly modifying the coset enumeration code to allow it to return an incomplete `CosetTable` (instead of raising a `ValueError` if, say, the maximum number of cosets is exceeded) and to start running with a partially filled table (which could be passed as a keyword argument) rather than with a new, empty instance. This is necessary as finding the presentation requires setting some of the coset table entries manually and adding relators to the future presentation while examining the undefined table entries, after which coset enumeration could be used to make more deductions. Messing with coset enumeration is the only complicated part (as far as I can see): the rest is pretty much sorted by now. I suppose, the only other thing is deciding when to find the presentation in one step and when to use the multi-step version (finding the presentation of a subgroup first and then the whole presentation based on that). The latter can be more efficient for larger groups. But I could think about that later, maybe run some tests, once the coset enumeration is set up properly. 
