---
title: Route Matching
---

Route matching follows a consistent and predictable pattern. This guide will explain how route trees are matched.

When TanStack Router creates your router, all of your routes are automatically resorted to match the most specific routes first. This means that regardless of the order your route tree is defined, routes will always be sorted to this order:

- Index Route
- Static Routes (most specific to least specific)
- Dynamic Routes (longest to shortest)
- Splat/Wildcard Routes

Consider the following pseudo route tree:

```
Root
  - blog
    - $postId
    - /
    - new
  - /
  - *
  - about
  - about/us
```

After sorting, this route tree will become:

```
Root
  - /
  - about/us
  - about
  - blog
    - /
    - new
    - $postId
  - *
```

This final order represents the order in which routes will be matched based on specificity.

Using that route tree, let's follow the matching process for a few different URLs:

- `/blog`
  ```
  Root
    ❌ about
    ⏩ blog
      ✅ /
      - new
      - $postId
    - /
    - *
  ```
- `/blog/my-post`
  ```
  Root
    ❌ about
    ⏩ blog
      ❌ /
      ❌ new
      ✅ $postId
    - /
    - *
  ```
- `/`
  ```
  Root
    ❌ about
    ❌ blog
      ❌ /
      ❌ new
      ❌ $postId
    ✅ /
    - *
  ```
- `/not-a-route`
  ```
  Root
    ❌ about
    ❌ blog
      - /
      - new
      - $postId
    ❌ /
    ✅ *
  ```

## Pathless Layout Route Matching

During matching, [pathless layout routes](../route-paths#pathless-layout-routes) are treated as if they are flat. If a route is not found in a pathless layout route's children, matching will continue out of the pathless layout route's children and on through the rest of the parent subtree like normal.
