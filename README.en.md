# Sliced File Structure

> Життя переможе смерть, а світ – темряву. 🇺🇦

**Sliced FS** is a methodology for developing a project file structure. It is based on the experience of developing React applications, but suitable for any JavaScript projects.

> **Translation into other languages**
>
> If you have the opportunity, please contribute and help to translate this document into other languages!

## Contents

1. [Problem](#problem)
1. [Solution](#solution)
1. [File structure example](#file-structure-example)
1. [Quick start](#quick-start)
1. [Concept](#concept)
   - [Supergroup](#supergroup)
   - [Group](#group)
   - [Unit](#unit)
   - [Extra](#extra)
1. [FAQ](#faq)
1. [Feedback](#feedback)
1. [Contributors](#contributors)

## Problem

Most often, projects use a **simple file structure**, in which files are grouped by type.
For example, the file structure of a React application might look like this:

> ```
> src
> |-- api
> |-- assets
> |-- components
> |-- contexts
> |-- helpers
> |-- hooks
> |-- values
> ```

Files and file directories related to components are placed in `components`, helpers in `helpers`, values in `values`, etc.

In **small projects** a simple file structure is usually suitable and doesn't cause problems.

But in **medium to large** projects, as the number of files and directories increases, difficulties begin to arise:

- **File and directory name collision has to be dealt with**

  Since there are a lot of resources in medium to large projects, sooner or later the problem of name collisions arises. Usually it is solved in two ways.

  The first way is to keep resources at the same nesting level, but complicate their names. For example, add prefixes.
  
  The disadvantage of this approach is that long lists of files and directories are formed over time. They are **long to scroll through**, also it is **difficult to find** what you need. And complex names are **less convenient to read** and **refer to them**.
  
  > ```
  > components
  > |-- (20 directories, beginning with "editor")
  > |-- (10 directories, beginning with "emoji
  > |-- (50 directories, beginning with "home")
  > |-- (30 directories, beginning with "post")
  > ```

  The second way is to use multiple levels of nesting.
  
  The disadvantage of this approach is that uncontrolled nesting occurs. It **complicates navigation** through the project and **evaluation of the project complexity and individual functions**.

  > ```
  > # looks quite simple, right?
  >
  > components
  > |-- editor
  > |-- emoji
  > |-- home
  > |-- post
  > ```

  > ```
  > # oops!
  >
  > components
  > |-- editor
  > |   |-- actions
  > |   |-- editor
  > |   |-- text-area
  > |-- emoji
  > |   |-- picker
  > |-- home
  > |   |-- layout
  > |   |-- posts
  > |       |-- create
  > |       |   |-- editor
  > |       |-- layout
  > |-- post
  >     |-- actions
  >     |-- content
  >     |   |-- content
  >     |   |-- image
  >     |   |   |-- layout
  >     |   |   |-- content-image
  >     |   |-- text
  >     |   |   |-- layout
  >     |   |   |-- content-text
  >     |   |-- video
  >     |       |-- layout
  >     |       |-- content-video
  >     |-- likes
  >     |-- profile-image
  >     |-- reply
  >         |-- editor
  >         |-- layout
  > ```

- **Difficult to trace links between resources** 

  If you're working on a certain feature in a medium to large project, it's hard to **understand which files belong to this feature and where they are located**. This happens due to weak grouping of files. More often, the only option is to study the source code and form an idea in your head.

  This problem is getting worse to varying degrees, depending on which way you choose to deal with name collisions - complex names and long lists or nested directories.

## Solution

**Sliced Design** introduces several terms (**units**, **groups**, **supergroups**) and defines some rules to solve this problem.

Advantages of **Sliced Design**:

- suitable for any JavaScript projects 🌠
- easy to understand and use, simple terms and rules  🍬
- solves the problem of file and directory name collisions 🎯
- no complicated names and long lists 🍝
- no uncontrolled nesting 🪆
- provides strong file grouping 📎
- fixed physical nesting throughout the project - maximum 3 levels 📌
- the ability to visually examine which files belong to a particular function of the application, without examining the source code 🧩
- the ability to visually assess the complexity of the project as a whole and individual functions 🎇

## File structure example

The file structure of a React project developed using **Sliced Design** might look like this:

> ```
> # src contains supergroups, they group units by function
>
> src
> |-- editor
> |-- emoji
> |-- home
> |-- home.posts
> |-- home.posts.create
> |-- home.shared
> |-- post
> |-- post.reply
> |-- post.shared
> |-- shared
> ```

> ```
> # supergroups contain groups, they group units by type
>
> src
> |-- home
>     |-- components
>     |-- helpers
>     |-- index.js
> ```

> ```
> # units can be directories with files
>
> src
> |-- home
>     |-- components
>         |-- layout
>             |-- component.jsx
>             |-- index.js
>             |-- styles.js
>
> # or just files
>
> src
> |-- home
>     |-- helpers
>         |-- page.js
> ```

You can see the full example at [CodeSandbox](https://codesandbox.io/s/sliced-design-react-9qstoo).

## Quick start

> For those who don't like boring theory. 🚀

### Properties

<table>
  <tr>
    <th></th>
    <th>Supergroup</th>
    <th>Group</th>
    <th>Unit</th>
  </tr>
  <tr>
    <td><b>Meaning</b></td>
    <td>Directory, groups units <b>by function</b></td>
    <td>Directory, groups units <b>by type</b></td>
    <td>File directory or file containing code and other resources</td>
  </tr>
  <tr>
    <td><b>Расположение</b></td>
    <td>In <code>src</code></td>
    <td>Inside supergroups</td>
    <td>Inside groups</td>
  </tr>
  <tr>
    <td><b>Nesting</b></td>
    <td colspan="3" align="center">Physically absent, <b>expressed at the level of names</b></td>
  </tr>
</table>

### Rules

<table>
  <tr>
    <th>What it is applied to</th>
    <th>Rule</th>
  </tr>
  <tr>
    <td>Supergroups / units (except unit-files)</td>
    <td><b>Centralized export</b> is required - the structure must contain an index module, any access to its resources must be carried out through it.</td>
  </tr>
  <tr>
    <td>Supergroups</td>
    <td>In any module of a supergroup, it is allowed to import resources from another supergroup of <b>the 1st nesting level</b>, taking into account centralized export.</td>
  </tr>
  <tr>
    <td>Groups</td>
    <td>In any group module, it is allowed to import resources from another group of <b>the 1st nesting level</b>, within the same supergroup.</td>
  </tr>
  <tr>
    <td>Nested supergroups</td>
    <td>In any supergroup module, <b>upward import</b> is allowed - resources from a child supergroup can be imported into the parent, taking into account centralized export.</td>
  </tr>
  <tr>
    <td>Nested groups</td>
    <td>In any group module, <b>upward import</b> is allowed - resources from a child group can be imported into a parent group, within the same supergroup.</td>
  </tr>
  <tr>
    <td>Nested supergroups / groups</td>
    <td>If resources need to be accessed simultaneously in the parent and child structures, they must be located in the structure. <code>*.shared</code>.</td>
  </tr>
</table>

### Exceptions

- **Supergroups `*.shared`**

  Used to store resources that need to be accessed simultaneously in the parent and child supergroups.

  - resources can be used in the parent supergroup and all nested in it
  - no centralized export rule
  - cannot have their own nested supergroups

- **Groups `*.shared`**

  Used to store resources that need to be accessed simultaneously in the parent and child groups.

  - resources can be used in the parent group and all nested in it
  - cannot have their own nested groups

## Concept

> For those who are interested in details and examples. 🔎

### Supergroup

Directory, groups units **by function**. Creates a new namespace.

> ```
> # supergroup example
>
> src
> |-- editor
>     |-- components
>     |-- values
>     |-- index.js
> ```

#### Location

Supergroups are located in the `src` directory.

#### Nesting

Supergroups can be nested. Nesting is expressed at the level of **directory names**, by listing the chain of parent supergroups separated by a dot.

> ```
> src
> |-- home
> |-- home.posts
> |-- home.posts.create
> |-- post
> |-- post.reply
> ```

#### Rules

- Supergroups require **centralized export**. The supergroup must contain an index module, any access to its resources must be made through it.

  > <code>src/<b>editor</b>/components/actions/component.jsx</code>:
  >
  > ```js
  > // ✅ it's possible
  >
  > import { Picker } from "../../../emoji";
  >
  > // ❌ you can't do that, direct access to supergroup resource
  >
  > import Picker from "../../../emoji/components/picker";
  > ```

- In any module of a supergroup, it is allowed to import resources from another supergroup of **the 1st nesting level**, taking into account centralized export.

  > <code>src/<b>home</b>/components/layout/component.jsx</code>:
  >
  > ```js
  > // ✅ it's possible
  >
  > import Post from "../../../post";
  >
  > // ❌ you can't do that, access to the supergroup of the 2nd level of nesting
  >
  > import PostReply from "../../../post.reply";
  > ```

- In any module of a supergroup, **upward import** is allowed - resources from a child supergroup can be imported into the parent, taking into account centralized export.

  > <code>src/<b>home</b>/components/layout/component.jsx</code>:
  >
  > ```js
  > // ✅ it's possible
  >
  > import Posts from "../../../home.posts";
  > ```

  > <code>src/<b>home.posts</b>/components/layout/component.jsx</code>:
  >
  > ```js
  > // ❌ you can't do that, import from parent supergroup to child
  >
  > import { HomeContext } from "../../../home";
  >
  > // ✅ but it's possible
  >
  > import HomeContext from "../../../home.shared/contexts/home";
  > ```

- If resources need to be accessed simultaneously in the parent and child supergroups, they must be located in the `*.shared` supergroup.

#### Exceptions

- **`*.shared` supergroups**

  Used to store resources that need to be accessed simultaneously in the parent and child supergroups.

  - resources can be used in the parent supergroup and all nested in it
  - no centralized export rule
  - can't have their own nested supergroups

  > <code>src/<b>home.posts</b>/components/layout/component.jsx</code>:
  >
  > ```js
  > // ✅ it's possible
  >
  > import HomeContext from "../../../home.shared/contexts/home";
  >
  > // ❌ you can't do that, `home.posts` supergroup does not have access to the resources of `post.shared` supergroup
  >
  > import PostContext from "../../../post.shared/contexts/post";
  > ```

### Group

Directory, groups units **by type**. Creates a new namespace.

> ```
> # group example
>
> src
> |-- editor
>     |-- components
>         |-- actions
>         |-- editor
>         |-- text-area
> ```

#### Location

Groups are located within supergroups.

#### Nesting

Groups can be nested. Nesting is expressed at the level of **directory names**, by listing the chain of parent groups separated by a dot.

> ```
> src
> |-- post
>     |-- components
>     |-- components.content.image
>     |-- components.content.text
>     |-- components.content.video
>     |-- helpers
>     |-- helpers.player
> ```

#### Rules

- In any group module, import of resources from another group of **the 1st nesting level** is allowed within the same supergroup.

  > <code>src/post/<b>components.content.video</b>/content-video/component.jsx</code>:
  >
  > ```js
  > // ✅ it's possible
  >
  > import player from "../../helpers/player";
  >
  > // ❌ you can't do that, access to the group of the 2nd level of nesting
  >
  > import player from "../../helpers.player/player";
  > ```

- In any group module, **upward import** is allowed - resources from a child group can be imported into a parent group, within the same supergroup.

  > <code>src/post/<b>helpers</b>/player.js</code>:
  >
  > ```js
  > // ✅ it's possible
  >
  > import player from "../helpers.player/player";
  > ```

  > <code>src/post/<b>helpers.player</b>/playback.jsx</code>:
  >
  > ```js
  > // ❌ you can't do that, import from parent group to child
  >
  > import math from "../helpers/math";
  >
  > // ✅ but it's possible
  >
  > import math from "../helpers.shared/math";
  > ```

- If resources need to be accessed simultaneously in the parent and child groups, they must be located in the `*.shared` group.

#### Exceptions

- **`shared` groups**

  Used to store resources that need to be accessed simultaneously in the parent and child groups.

  - resources can be used in the parent group and all nested in it
  - can't have their own nested groups

  > <code>src/post/<b>components</b>/content/component.jsx</code>:
  >
  > ```js
  > // ✅ it's possible
  >
  > import Backdrop from "../../components.shared/backdrop";
  >
  > // ❌ you can't do that, `components` group does not have access to the resources of `helpers.shared` group
  >
  > import math from "../../helpers.shared/math";
  > ```

### Unit

File directory or file that contains code and other resources.

> ```
> # units example
>
> # unit-directory
>
> src
> |-- editor
>     |-- components
>         |-- text-area
>             |-- component.jsx
>             |-- index.js
>             |-- styles.js
>
> # unit-file
>
> src
> |-- editor
>     |-- values
>         |-- limits.js
> ```

#### Location

Units are located within groups.

#### Nesting

Files within a unit can be nested. Nesting is expressed at the level of **filenames**, by listing the chain of parent directories separated by a dot.

> ```
> src
> |-- editor
>     |-- components
>         |-- actions
>             |-- assets.emoji-icon.svg
>             |-- assets.gif-icon.svg
>             |-- assets.media-icon.svg
>             |-- component.jsx
>             |-- index.js
>             |-- styles.js
> ```

#### Rules

- Units require **centralized export** (except for unit-files). The unit must contain an index module, any access to the resources located inside must be carried out through it.

  > <code>src/editor/components/<b>editor</b>/component.jsx</code>:
  >
  > ```js
  > // ✅ it's possible
  >
  > import Actions from "../actions";
  >
  > // ❌ you can't do that, direct access to unit resources
  >
  > import mediaIcon from "../actions/assets.media-icon.svg";
  > ```

### Extra

- It is recommended to use kebab-case for naming directories and files.

## FAQ

**Can Sliced Design be used with TypeScript?**

Yes!

**Can Sliced Design be used with CommonJS?**

Yes! Only the syntax of the modules will be different.

**Can Sliced Design be used with other languages?**

I guess so! Only the module system in your language may differ significantly. If you succeed, please share your experience.

## Feedback

I will be glad to any feedback! Please post it in discussions or issues.

## Contributors

- [@austrokhart](https://github.com/austrokhart)
- [@daryabratova](https://github.com/daryabratova)
- [@a1ex-kaufmann](https://github.com/a1ex-kaufmann)
- [@kindaro](https://github.com/kindaro)

## Support the author

[Boosty](https://boosty.to/austrokhart)
