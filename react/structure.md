---
title: React File Structure
---

# File Structure

There should be only specific sub-folders at the root of the src folder - for very specific purposes.

## APIs

This folder purpose is to provide reusable elements pertaining to a specific API use - whether or not it is REST of GraphQL.

For REST, it would be exporting a reusable client. For GraphQL, it would be export the GraphQL definition of the queries and mutations.

As much as possible, it should be limited to 1 file per API, exporting all its context as named exports.

If required, we can use a sub-folder instead to split into multiple files, with a central index.ts file exporting all the named exported of the siblings files.

```
apis/
  - media.ts
  - talent.ts
  - google/
    - geolocation.ts
    - ...
    - index.ts
  - ... 
```

## Assets

Depending on the build configuration, we may need to store assets like images, fonts, and more, outside of the framework public folder.

This should be kept to assets that are not served over HTTP from the public folder - or to assets that are transformed / optimized by the build step.

The best structure option is to split per asset type:

```
assets/
  - img/
    - ...
  - fonts/
    - ...
```

## Components

All and every components should be listed in this folder. This is the single place to find any and all components.

Components folders should contain a file bearing the same name as the exported component, a file for styling, if relevant a file for the component stories, and an index file for convenience.

```
components/
  - Button/
    - index.ts
    - Button.tsx
    - Button.module.scss (web)
    - Button.style.ts (mobile)
    - Button.stories.tsx
```

In case of small project, we can keep a flat structure where all the components are listed at the root.

But for larger projects where a significant amount of components are needed and are usually grouped by scope, we should organize them by sub-folders representing scope.

For instance, if we have a User root scope, and have a component for UserThumbnail, alongside other components required for User - we can nest it like this:

```
components/
  - User/
    - Thumbnail/
      - UserThumbnail.tsx
      - ...
  - Button/
    - ...
```

Note that the name of the component (both name of the file and name of the export) should include all the scopes as well.

✅ DO 
- `<UserThumbnail /> `
- `User/Thumbnail/UserThumbnail.tsx`

❌ DO NOT
- `<Thumbnail />`
- `User/Thumbnail.tsx`

There should never be nested components inside another component folder.

✅ DO
```
components/
  - Scope1
    - Scope2
      - ComponentA
        - Scope1Scope2ComponentA.tsx
        - ...
      - ComponentB
        - Scope1Scope2ComponentB.tsx
        - ...
```

❌ DO NOT
```
components/
  - Scope1/
    - ComponentA/
      - Scope1ComponentA.tsx
      - ...
      - Scope2/
        - ComponentB/
          - Scope1ComponentAScope2ComponentB.tsx
          - ...
```

## Contexts (web only)

Contexts should be use on web to provide downstream components with state and actions based on path and scope - instead of using a global persistent store like redux.

We can split contexts into 2 main categories: lists and item.
- Lists contexts will provide mostly read, filter and sort capabilities for a collection of items.
- Item contexts will provide full CRUD capabilities on a single item.

```
contexts/
  - talent/
    - item.tsx
    - list.tsx
  - project/
    - item.tsx
    - list.tsx
```

## Hooks

Hooks are the main way to interact with APIs and should provide a common interface following the `<data>`, `<loading>`, `<error>` pattern.

Hooks should mainly be used by Contexts or Redux, and should export specific actions.

For GraphQL, it will match the queries and mutations offered the server, or we could expose new hooks that leverage several queries or mutations together.

We can split into folders if the number of hooks grows too big for a single file. In that case we should keep them grouped by scope.

```
hooks/
  - talent.ts
  - project/
    - info.ts
    - deliverable.ts
```

## Navigation (mobile only)

The navigation folder in mobile is there to handle routing and deep linking within the app.

The structure should be fairly simple and should not grow much over time.

The main file is the router - which is the one providing the app navigator - and holding the routing and deep linking logic.

We can split the routes into a sub folder as needed. The point is group them per scope or requirements and to de-clutter the router so that it focuses on the main logic and not the routes listing and rendering.

```
navigation/
  - router.tsx
  - routes/
    - public.tsx
    - private.tsx
```

## Pages / Screens

Pages and Screens are basically root components for our application. They should be closely tied to navigation, and path (on web).

On Web, pages should be responsible to provide Context to all components under them, interface with the router.

Structurally, the page sub-folders are structured in a similar way than then components, with some variations.
pages/ (web) or screens/ (mobile - page would be replaced by screen in the path or file names)

```
  - user/
    - index.page.tsx
    - UserPage.module.scss (web)
    - UserPage.style.ts (mobile)
    - UserPage.stories.tsx
    - [userId]/
      - index.page.tsx
      - UserDetailPage.module.scss (web)
      - UserDetailPage.style.ts (mobile)
      - UserDetailPage.stories.tsx
```

The structure of the pages folder should follow the structure of the path when accessing the page.

Following that logic, `pages/user/[userId]/index.page.tsx` would be accessed by going to `/user/aaaaaaaa-bbbb-cccc-dddd-1234567890abcdef/`

Contrary to components, the name of the file will almost always be index.page.tsx - as a reference to index file in web servers, the rest of the support files (styling and stories) should still be named after the main export (component name).

## Redux

Leveraging Redux ToolKit (RTK), we can simplify the structure into the store set up, and simple reducers declarations.

This is expected to stay simple on web - as it will be used only for global elements like user preferences, toast messages and notifications - but the slice folders allows it to grow on mobile without getting messy.

```
redux/
  - store.ts
  - user/
    - slice.ts
  - notification
    - slice.ts
```

## Utils

The utils section purpose is to provide reusable functions that are generic. By generic, we mean that is not specific to a part of the application, or not producing content only usable by a part of the application.

We should think of that section as internal packages to achieve a very specific thing.

Good example of utils are:
- Formatting string like prices, dates etc.
- Services used throughout the app (analytics, bug reporting, logging, etc.)

```
utils/
  - analytics.ts
  - formatting/
    - price.ts
    - date.ts
```
