---
title: AI as an abstraction
author: LQR471814
date: 2025-11-30
categories:
- thoughts
tags:
- programming
- ai
---

I feel like AI, in regard to programming, is simply another
abstraction layer for programming.

You specify the abstraction using natural language, documentation,
and programming jargon, it outputs all (or most) of the necessary
correct changes to implement the abstract contract you specified
in the context.

This is not unlike classical abstraction (ex. a programming
language, a library) which explicitly defines (using some formal
system) how the pieces in the lower layer come together based on
the contract specified in the abstraction layer.

> [!EXAMPLE]
> A python library that handles matrix multiplication abstracts
> away the specific calculations or SIMD calls you must make to
> compute the actual result. Garbage collection abstracts away the
> logic of freeing unused memory. Etc...

Abstraction does not really matter to the computer, in fact, in
many instances (though not all), programs built with abstractions
tend to have suboptimal performance (ex. garbage collection,
higher-level languages, libraries/frameworks, etc...) The main
gain of abstraction is lowered human effort.

By sharing an abstraction you reduce duplicated work on the human
end, and the time it takes to get from idea to working software in
human time. (ex. function definition, modules, packages,
frameworks, etc...) In other words, you can specify more with
less.

> [!NOTE]
> Of course, the abstractions which currently exist do not allow
> you to build everything that exists. There are many instances
> where the abstractions that exist only partially fit your use
> case, or do not do so at all, so you end up building a solution
> specific to your use-case (or adapting an existing solution with
> a host of patches depending on the cost-benefit analysis).

Any software entails a formal specification of semantics. (ie.
code, configuration, etc...) And a correct program is often a
formal reflection of various external specifications of correct
behavior (formal or informal).

Abstractions carve out chunks of the total length of this formal
specification and outsource it to the abstraction. These are often
components that are weakly-coupled in the whole system and shared
between many different systems. (ex. database, network layer, FS,
well-known algorithms, etc...)

This is because:

1. It is easy to verify their correctness when their dependencies
   and interfaces are more easily defined.
2. It is more cost-efficient, the more widely they are shared.

In contrast, abstractions often break down when these two
constraints are violated, ie:

- When dependencies (things that can influence program behavior)
  are many, resulting in complex and potentially fragile
  implementations.
- When the abstraction is not shared often. (that is, when the
  cost does not justify the benefit)

This can be seen in:

- Traditional no-code/low-code platforms:
    - Traditional no-code/low-code platforms bring together many
      components (database, web ui components, business logic
      components) to implement its functionality. Those are its
      dependencies.
        - This means that correctness is hard to ensure.
        - To address this, no-code/low-code platforms have to make
          compromises on the number of problems they can solve.
          Oftentimes, this puts limits on the plausible use-cases
          for no-code/low-code platforms (ex. basic sites and
              admin dashboards).
        - More complex use-cases entail a necessary increase of
          the complexity in specification, as well as a necessary
          decrease in the guarantees of correctness.
    - However, no-code/low-code is viable because of the
      economics, simply put, there are a lot of people who just
      want a simple static site or admin dashboard.
        - If that is all one wants, it makes complete sense to use
          no-code/low-code as an abstraction to quickly get the
          job done. Even if a more optimal solution (as in
          performance, UX) could be achieved through careful
          design and a custom implementation using a lower-level
          abstraction.
        - However, in many cases (and the reason why
          no-code/low-code isn't everywhere) peoples' use cases
          will expand, and custom logic is inevitable.
- Any large build system:
    - Anyone who has ever used a large complex build system that
      depends on an ecosystem of tools will know that it is almost
      constantly broken. (ex. think about JavaScript/Web/Node.js
      ecosystem circa 2015-2020, legacy C/C++, Python before
      poetry and other modern tooling, anything touching
      cmake/autoconf/bazel/etc...)
    - You can think of a build system in general as a giant
      program that often invokes many tools to turn your code (and
      possibly many intermediaries) into the things you will
      deploy.
    - In this sense, the dependencies are all the compilers (and
      their versions), the includes, shared libraries, and
      packages (implicit or explicit), the OS and CPU
      architecture, etc...
        - It is not hard to see why correctness is hard to ensure
          in a large and complex build system.
        - All the tools you depend on in your build system will
          often change out of the hands of a single governing body
          (either you or some other authority) and if even one
          tool does something unexpected, the whole chain will
          fail.
        - Even if you pin versions and verify hashes, you will
          often need to upgrade your tools just to attain latest
          features and enhancements, this will necessarily require
          you to deal with breakage either way.
    - That said, it is often the case that having working software
      (albeit with a bit more effort as a result of maintaining
      the build system) is much better than not having any
      software at all. So in certain platforms, you would simply
      deal with suboptimal build system abstractions. (ex. C/C++)


