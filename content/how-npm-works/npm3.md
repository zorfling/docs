<!--
title: 03 - npm v3
featured: true
-->

# npm v3 Dependency Resolution

npm3 resolves dependencies differently than npm2.

While npm2 installs all dependencies in a nested way, npm3 tries to
mitigate the deep trees and redundancy that such nesting causes. npm3
attempts this by installing some secondary dependencies (dependencies
of dependencies) in a flat way, in the same directory as the primary
dependency that requires it.

The key major differences are:

  - position in the directory structure no longer predicts the type
    (primary, secondary, etc) a dependency is
  - dependency resolution depends on *install order*, or the order
    in which things are installed will change the `node_modules`
    directory tree structure

## Example - <a class="button" href="https://github.com/ashleygwilliams/npm-sandbox/tree/master/npm3/example1">Explore on Github</a>

Imagine we have a module, A. A requires B.

![A depends on B](/public/images/npm3deps1.png)

Now, let's create an application that requires module A.

On `npm install`, npm v3 will install both module A and its
dependency, module B, inside the `/node_modules` directory, flat.

In npm v2 this would have happened in a nested way.

![npm2 vs 3](/public/images/npm3deps2.png)

Now, let's say we want to require another module, C. C requires B,
but at another version than A.

![new module dep, C](/public/images/npm3deps3.png)

However, since B v1.0 is already a top-level dep, we cannot install
B v2.0 as a top level dependency. npm v3 handles this by defaulting
to npm v2 behavior and nesting the new, different, module B version
dependency under the module that requires it -- in this case, module C.

![nested dep](/public/images/npm3deps4.png)

In the terminal, this looks like this:

![tree](/public/images/tree.png)

You can list the dependencies and still see their relationships using
`npm ls`:

![npmls](/public/images/npmls.png)

If you want to just see your primary dependencies, you can use:

```
npm ls --depth=0
```

![npmlsdepth0](/public/images/npmlsdepth0.png)
