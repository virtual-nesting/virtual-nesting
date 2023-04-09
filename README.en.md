# Virtual Nesting 👩‍💻

> Життя переможе смерть, а світ – темряву. 🇺🇦

**Virtual Nesting** is a new approach to developing **file structure** projects. Makes it easier to work with JavaScript projects.

> **Translation into other languages**
>
> If you have the opportunity, please contribute and help translate this document into other languages!

## Contents

1. [Poorly organized file structure is a problem](#poorly-organized-file-structure-is-a-problem)
1. [Current approaches to organization have significant flaws](#current-approaches-to-organization-have-significant-flaws)
    - [Folders-by-type](#folders-by-type)
    - [Folders-by-feature](#folders-by-feature)
    - [Nesting doesn't solve the problem either](#nesting-doesn't-solve-the-problem-either)
1. [The solution is Virtual Nesting](#the-solution-is-virtual-nesting)
1. [Quick start](#quick-start)
    - [Brief review](#brief-review)
    - [Apply step by step](#apply-step-by-step) 🔥
    - [Project example](#project-example)
1. [Methodology](#methodology)
    - [Category](#category)
      - [Category location](#category-location)
      - [Subcategories](#subcategories)
      - [Category restrictions](#category-restrictions)
      - [Meta-categories](#meta-categories)
    - [Group](#group)
      - [Group location](#group-location)
      - [Subgroups](#subgroups)
      - [Group restrictions](#group-restrictions)
      - [Meta-groups](#meta-groups)
    - [Element](#element)
      - [Element location](#element-location)
      - [Module links within a directory element](#module-links-within-a-directory-element)
      - [Element restrictions](#element-restrictions)
    - [Resource](#resource)
    - [Compatibility with architectural patterns](#compatibility-with-architectural-patterns)
    - [General recommendations](#general-recommendations)
1. [Notes](#notes)
1. [FAQ](#faq)
1. [Feedback](#feedback)
1. [Contributors](#contributors)
1. [License](#license)

## Poorly organized file structure is a problem

Any project is a collection of program code. As the project develops, the **volume** of this code and the **complexity** of working with it grow.

The way how the complexity will grow depends largely on the decisions made during development.

If decisions are made **situationally**, the difficulty will grow uncontrollably and most likely rapidly. At the same time, if you find the best approaches, develop a **system** of rules and stick to it, the increase in complexity can be controlled.

One of the most difficult tasks when working with projects is working with **modules**, understanding their **purposes** and **links** between them.

To make it easier to understand, when working with **code**, one stick to the **architecture** and **conventions** accepted in the project, which correlate with popular patterns, practices and individual experience.

But the code can't exist on its own. In order to become part of a project, it must be placed in a **file**. Files form a file structure. In it, **name** and directory **membership** help to understand the **purpose** of a file and **links** to other files.

The **poorly organized** file structure **complicates** this understanding. Moreover, since most of the files in projects are modules, this also makes working with **modules** more difficult, understanding their **purpose** and the **relationships** between them. It is one of the most difficult tasks when working with projects.

From this we can conclude that **poorly organized file structure is a problem**.

## Current approaches to organization have significant flaws

There are two popular approaches to organize the file structure: 

- [**folders-by-type**](#folders-by-type)
- [**folders-by-feature**](#folders-by-feature)

### Folders-by-type

This approach involves grouping files by type.

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

Files related to components are placed in `components`, helpers in `helpers`, values in `values`, etc.

The approach is sufficient in small projects, but with a large number of files there are **problems**:

- **Difficult to navigate, complicated filenames**

  > ```
  > components
  > |-- (15 files beginning with prefix "app")
  > |-- (10 files beginning with prefix "emoji")
  > |-- (20 files beginning with prefix "home")
  > |-- (10 files beginning with prefix "layout")
  > |-- (30 files beginning with prefix "post")
  > |-- (20 files beginning with prefix "post-editor")
  > |-- (15 files beginning with prefix "trends")
  > |-- ...
  > ```

  Since a group can contain hundreds of files in a medium to large project, long lists are formed that can be **difficult to navigate**.
  
  Prefixes are added to file or directory names to avoid name collisions. Such names can be **inconvenient to read** and **refer to them**.

- **Difficult to understand if a file belongs to a specific function**

  Because the files are only grouped by type, it's often only possible to figure out which files belong to a particular function **by examining the code**.

### Folders-by-feature

The approach involves grouping files according to their belonging to a function.

> ```
> src
> |-- app
> |-- emoji
> |-- home
> |-- layout
> |-- post
> |-- trends
> ```

Files related to the home page are placed in `home`, posts — in `post`, etc.

This approach solves the main problem of the **folders-by-type** approach (lack of grouping by belonging to a function), but with a large number of files, **problems** also arise:

- **Difficult to navigate**

  > ```
  > src
  > |-- post
  > |   |-- actions-like.jsx
  > |   |-- actions-like.styles.js
  > |   |-- actions-reply.jsx
  > |   |-- actions-reply.styles.js
  > |   |-- actions-repost.jsx
  > |   |-- actions-repost.styles.js
  > |   |-- actions-share.jsx
  > |   |-- actions-share.styles.js
  > |   |-- content-image.jsx
  > |   |-- content-image.styles.jsx
  > |   |-- content-text.jsx
  > |   |-- content-text.styles.js
  > |   |-- content-video.jsx
  > |   |-- content-video.styles.js
  > |   |-- content.jsx
  > |   |-- content.styles.js
  > |   |-- image-viewer-backdrop.jsx
  > |   |-- image-viewer-backdrop.styles.js
  > |   |-- image-viewer-navigation.jsx
  > |   |-- image-viewer-navigation.styles.js
  > |   |-- image-viewer.jsx
  > |   |-- image-viewer.styles.js
  > |   |-- post.jsx
  > |   |-- post.styles.js
  > |   |-- views.jsx
  > |   |-- views.styles.js
  > |
  > |-- post-editor
  > |   |-- audience.jsx
  > |   |-- audience.styles.js
  > |   |-- editor.jsx
  > |   |-- editor.styles.js
  > |   |-- emoji.jsx
  > |   |-- emoji.styles.js
  > |   |-- media.jsx
  > |   |-- media.styles.js
  > |   |-- poll.jsx
  > |   |-- poll.styles.js
  > |   |-- post-limits.constants.js
  > |   |-- settings.jsx
  > |   |-- settings.styles.js
  > |   |-- text-area.jsx
  > |   |-- text-area.styles.js
  > |
  > |-- ...
  > ```

  Since a group can contain dozens of files in a medium or large project.  The files are of **mixed type**, so it is **significantly** more difficult to navigate. When looking at a directory, we know that the files belong to a certain function, but it is hard for us to understand **what type they are** and **how many there are**.

### Nesting doesn't solve the problem either

Nesting solves the problem of name collisions, but **significantly worsens** the understanding of the relationship between modules, since some parts of the project are **not visible** until all nested directories are expanded.

> ```
> # let's examine the `post` directory 👀
>
> src
> |-- post
> |-- ...
> ```
>
> ```
> # looks quite simple, but there are several nested directories
>
> src
> |-- post
> |   |-- actions
> |   |-- content
> |   |-- image-viewer
> |   |-- post.jsx
> |   |-- post.styles.js
> |   |-- views.jsx
> |   |-- views.styles.js
> |-- ...
> ```
>
> ```
> # oops!
>
> src
> |-- post
> |   |-- actions
> |   |   |-- like.jsx
> |   |   |-- like.styles.js
> |   |   |-- reply.jsx
> |   |   |-- reply.styles.js
> |   |   |-- repost.jsx
> |   |   |-- repost.styles.js
> |   |   |-- share.jsx
> |   |   |-- share.styles.js
> |   |-- content
> |   |   |-- content.jsx
> |   |   |-- content.styles.js
> |   |   |-- image.jsx
> |   |   |-- image.styles.jsx
> |   |   |-- text.jsx
> |   |   |-- text.styles.js
> |   |   |-- video.jsx
> |   |   |-- video.styles.js
> |   |-- image-viewer
> |   |   |-- backdrop.jsx
> |   |   |-- backdrop.styles.js
> |   |   |-- image-viewer.jsx
> |   |   |-- image-viewer.styles.js
> |   |   |-- navigation.jsx
> |   |   |-- navigation.styles.js
> |   |-- post.jsx
> |   |-- post.styles.js
> |   |-- views.jsx
> |   |-- views.styles.js
> |-- ...
> ```

⬆️ [Back to contents](#contents)

## The solution is Virtual Nesting

**Virtual Nesting** is a new approach to developing **file structure** projects. It introduces a system of terms ([categories](#category), [groups](#group), [elements](#element)) and rules to make working with JavaScript projects easier.

Advantages of **Virtual Nesting**:

- suitable for projects of **any** complexity 🚀
- makes it easier to develop and support projects 👩‍🔧
- easy to understand and use, simple terms and rules ✌️
- groups files by function and type at the same time  📦
- simplifies the understanding of the relationships between files 🎇
- solves the problem of file name collisions 🪆
- provides a fixed nesting of directories throughout the project - a maximum of 3 levels 📌

## Quick start

### Brief review

The file structure of a React project developed using the **Virtual Nesting** approach might look like this:

- `src` contains [**categories**](#category), they group [**elements**](#element) (files) by belonging to **function**

  > ```
  > # in the example below
  > # `app`, `emoji`, `home`, `layout`, etc. - categories
  >
  > src
  > |-- app
  > |   |-- ...
  > |-- emoji
  > |   |-- ...
  > |-- home
  > |   |-- ...
  > |-- layout
  > |   |-- ...
  > |-- post
  > |   |-- ...
  > |-- post.image-viewer
  > |   |-- ...
  > |-- post@shared
  > |   |-- ...
  > |-- post-editor
  > |   |-- ...
  > |-- trends
  > |   |-- ...
  > |-- ui-kit
  >     |-- ...
  > ```

- **categories** contain [**groups**](#group), they group [**elements**](#element) (files) by belonging to **type**

  > ```
  > # in the example below
  > # `layout` - category, `components` and `helpers` - groups
  >
  > src
  > |-- layout
  > |   |-- components
  > |   |   |-- ...
  > |   |-- helpers
  > |   |   |-- ...
  > |   |-- index.js
  > |-- ...
  > ```

- [**elements**](#element) can be **files** or **directories with files**

  > ```
  > # in the example below
  > # `layout` - category, `helpers` - group, `scroll.js` - element
  >
  > src
  > |-- layout
  > |   |-- helpers
  > |   |   |-- scroll.js
  > |   |   |-- ...
  > |   |-- ...
  > |-- ...
  > ```
  >
  > ```
  > # in the example below
  > # `layout` - category, `components` - group, `navigation` - element
  > 
  > src
  > |-- layout
  > |   |-- components
  > |   |   |-- navigation
  > |   |   |   |-- component.jsx
  > |   |   |   |-- index.js
  > |   |   |   |-- styles.js
  > |   |   |-- ...
  > |   |-- ...
  > |-- ...
  > ```

You can see a full example of the project's file structure [here](#project-example).

⬆️ [Back to contents](#contents)

### Apply step by step

Let's imagine that we are *a front-end developer with experience developing React applications* and we were invited to work in a project where a new social network is being developed.

The project is at a very early stage of development. We found ourselves in the role of lead developer and we have to initialize the front-end project.

#### Initializing the application

> *Here we create the first **category**, **group** and **element**, get acquainted with their properties and restrictions.*

We have initialized a new repository and are about to add our first files. Let's start by creating the root component and initializing the application.

First, let's create [category](#category) `app` in `src`.

> **Category** is a directory that is used to group files according to their belonging to a function.

In the `app` category we create [group](#group) `components`.

> **Group** is a directory that is used to group files by type.

In the group `components` we create [element](#element) `app`. 

> **Element** is a file or directory with files, it contains code.

In the element `app` we create `component.jsx` module with the component code and `index.js` module that re-exports the component from `component.jsx` module.

We get such a structure:

> ```
> src
> |--app
>    |-- components
>        |-- app
>            |-- component.jsx
>            |-- index.js
> ```

> **Important!** **Virtual Nesting** defines the structure of your project, but not the names of files and directories. All names are chosen by you, based on popular conventions and individual experience.

Now let's create a module with initialization code that will mount our root component. It also belongs to the `app` category, but to the `effects` group.

> ```
> src
> |--app
>    |-- components
>    |   |-- app
>    |       |-- component.jsx
>    |       |-- index.js
>    |-- effects
>        |-- init-app.js
> ```

Next, we need to specify the path to the initialization module in our module bundler.

We could have specified the path `src/app/effects/init-app.js`, but it should be taken into account that categories have a [restriction](#category-restrictions) on access to their internal resources.

> External access (from other categories) to internal category resources is allowed *only* through modules **at the root** of the category.
> 
> All resources that are planned to be used *from outside* (imported in other categories) should be *re-exported* in these modules.
> 
> This restriction provides **explicit** entry points to the category and provides **hiding** internal resources.

Therefore, we will create the `init-app.js` module in the category root, in which we import the `effects/init-app.js` module.

> ```
> src
> |--app
>    |-- components
>    |   |-- app
>    |       |-- component.jsx
>    |       |-- index.js
>    |-- effects
>    |   |-- init-app.js
>    |-- init-app.js
> ```

Now we can specify the path to the `src/app/init-app.js` module in the bundler, and it's done! 🎉

#### Starting work on the home page

Together with the manager, we decided that among the functions of the application, it is worth starting with the implementation of the home page.

We have just initialized the project, so we need to do some prep work.

In addition, this is a complex task, so we will divide it into several simpler ones.

- After examining the layouts, we understand that the home page uses elements common to the entire project, such as profile images, buttons, links, pop-ups. They are included in the UI kit developed by designers, we have its layouts and documentation.

  Let's start with the implementation of the **UI kit**. We implement the minimum set of components and states that we need, so that it would not take long.

- Then let's move on to the implementation of the **`Layout`** component common for pages, which will be responsible for laying out the content and rendering common elements - logo, navigation, links to the profile and information about the platform.

- The main page has features that will later be used on other pages - **posts**, **post editor** and **trends**. Let's create separate categories for them.

- Let's put it all together!

Let's get started! 👩‍🔧

#### Creating a UI kit and a `Layout` component

> *Here we get acquainted with different options for re-exporting resources from categories, examples of imports in other categories.*

Let's start developing the UI kit and the `Layout` component.

Let's create a `ui-kit` category with a `components` group in it. In it we will create elements `button`, `link`, `popover`, `profile-image`.

Categories have a restriction on access to internal resources, so let's create modules at the root that will re-export the necessary components.

```
src
|-- ui-kit
|   |-- components
|   |   |-- button
|   |   |   |-- component.jsx
|   |   |   |-- index.js
|   |   |   |-- styles.js
|   |   |-- link
|   |   |   |-- component.jsx
|   |   |   |-- index.js
|   |   |   |-- styles.js
|   |   |-- popover
|   |   |   |-- component.jsx
|   |   |   |-- index.js
|   |   |   |-- styles.js
|   |   |-- profile-picture
|   |       |-- component.jsx
|   |       |-- index.js
|   |       |-- styles.js
|   |-- button.js
|   |-- link.js
|   |-- popover.js
|   |-- profile-image.js
|-- ...
```

For example, importing the `ProfileImage` component in components of other categories would look like this:

```js
import ProfileImage from '../../../ui-kit/profile-image';
```

The minimal implementation of the UI kit is ready! 🎉

Now let's take a look at the `Layout` component.

Let's create a `layout` category. It contains a `components` group with `layout` elements, as well as `about`, `logo`, `navigation`, `profile`.

We will also create a `helpers` group with `scroll.js` and `window.js` elements. These helpers will be useful to us.

The main element of this category is `layout`. To avoid duplicating the name when importing (`layout/layout`), we re-export `layout` in the index module (`index.js`).

```
src
|-- layout
|   |-- components
|   |   |-- about
|   |   |   |-- component.jsx
|   |   |   |-- index.js
|   |   |   |-- styles.js
|   |   |-- layout
|   |   |   |-- component.jsx
|   |   |   |-- index.js
|   |   |   |-- styles.js
|   |   |-- logo
|   |   |   |-- component.jsx
|   |   |   |-- index.js
|   |   |   |-- styles.js
|   |   |-- navigation
|   |   |   |-- component.jsx
|   |   |   |-- index.js
|   |   |   |-- styles.js
|   |   |-- profile
|   |       |-- component.jsx
|   |       |-- index.js
|   |       |-- styles.js
|   |-- helpers
|   |   |-- scroll.js
|   |   |-- window.js
|   |-- index.js
|-- ...
```

For example, importing a `Layout` component in components of other categories would look like this:

```js
import Layout from '../../../layout';
```

Success! 🎉

#### Creating a `Post` component

> *Here we create the first **subgroup**, get acquainted with its properties and restrictions.*

Let's create a `post` category for the `Post` component.

```
# here and below the structure of the components is simplified

src
|-- post
|   |-- components
|   |   |-- content
|   |   |   |-- ...
|   |   |-- image
|   |   |   |-- ...
|   |   |-- like
|   |   |   |-- ...
|   |   |-- post
|   |   |   |-- ...
|   |   |-- reply
|   |   |   |-- ...
|   |   |-- repost
|   |   |   |-- ...
|   |   |-- share
|   |   |   |-- ...
|   |   |-- text
|   |   |   |-- ...
|   |   |-- video
|   |   |   |-- ...
|   |   |-- views
|   |       |-- ...
|   |-- index.js
|-- ...
```

Let's examine the resulting list of components. It can be divided into two subgroups:

- `content` — with components that are responsible for rendering content,
- `actions` — with components that are responsible for rendering actions with the post.

Additional grouping helps to better understand the purpose of the elements. Therefore, in this case, we will create [subgroups](#subgroups) `components#content` and `components#actions`.

> Groups can have subgroups. A subgroup is a directory that is used to **additionally group the elements of a group**.
> 
> The subgroup name has the structure: `<group name>#<subgroup name>`.
> 
> A subgroup differs from a regular group by having a **link with parent groups** (one or more) and an associated with this **restriction on access to resources**.

```
src
|-- post
|   |-- components
|   |   |-- post
|   |   |   |-- ...
|   |   |-- views
|   |       |-- ...
|   |-- components#actions
|   |   |-- like
|   |   |   |-- ...
|   |   |-- reply
|   |   |   |-- ...
|   |   |-- repost
|   |   |   |-- ...
|   |   |-- share
|   |       |-- ...
|   |-- components#content
|   |   |-- content
|   |   |   |-- ...
|   |   |-- image
|   |   |   |-- ...
|   |   |-- text
|   |   |   |-- ...
|   |   |-- video
|   |       |-- ...
|   |-- index.js
|-- ...
```

There is a [restriction](#group-restrictions) on access to subgroup resources:

> Access to subgroup resources is allowed only from **parent groups** and **groups with a different name** (for example, from `components` to `helpers#math`). Access in **subgroups** to resources of **parent groups** is *prohibited*.
>
> This restriction structures **links** between groups and subgroups and avoids **cyclic dependencies**.
>
> If resources *need* to be accessed simultaneously in the parent group and all of its subgroups, you must place them in the [meta-group](#meta-groups) @shared.

We can freely refer to `components` resources `components#actions` and `components#content`, but not vice versa.

Success! 🎉

#### Complicating `Post`, adding a new feature - full-screen image viewing

> *Here we create the first **subcategory** and **meta-category**, get acquainted with their properties and restrictions.*

The post display component is a complex feature. It can be broken down into simpler features, such as full-screen viewing of images attached to a post.

The full-screen view function is separate, but can't exist in isolation from the main function. In this case, for better grouping, it is worth separating them, but clearly indicating the relationship between them.

For this purpose, we create a [subcategory](#subcategories) `post.image-viewer`.

> Categories can have subcategories. A subcategory is a directory that is used for **decomposing complex functions**.
> 
> For example, when it would be better to break one **complex** function into several **simple** ones.
> 
> A subcategory differs from a regular category by having a **link with parent categories** (one or more) and an associated with this **restriction on access to resources**.

```
src
|-- post
|   |-- ...
|
|-- post.image-viewer
|   |-- components
|   |   |-- backdrop
|   |   |   |-- ...
|   |   |-- navigation
|   |   |   |-- ...
|   |   |-- viewer
|   |       |-- ...
|   index.js
|
|-- ...     
```

In the full-screen view, the actions with the post are duplicated - like, repost, reply, share.

We have already implemented the appropriate components in the `post` category and could refer to them, but the following should be taken into account: subcategories have a [restriction](#category-restrictions) on access to parent category resources.

> Access in **subcategories** to resources of **parent categories** is *prohibited*.
> 
> This restriction structures **links** between categories and subcategories and avoids **cyclic dependencies**.

Thus, we can't refer to the resources of the parent `post` category in the `post.image-viewer` subcategory.

But there is a solution - to create a [meta-category](#meta-categories) `post@shared`.

> A meta-category differs from a regular category in that:
>
> - access to meta-category resources is allowed in the **base category** (without meta-tag) and all its **subcategories** — and only in them,
> 
> - in the **meta-category** *forbidden* access to the resources of the **base category** and all of its **subcategories**,
> 
> - a meta-category can **override** regular category restrictions.
> 
> The name has the structure: `<base category name>@<meta tag>`.
>
> In the @shared meta-category, the restriction on direct access to internal resources **does not apply**, you can access resources *directly*.

Let's create a `post@shared` meta category and move the `components#actions` group into it.

```
src
|-- post
|   |-- components
|   |   |-- post
|   |   |   |-- ...
|   |   |-- views
|   |       |-- ...
|   |-- components#content
|   |   |-- content
|   |   |   |-- ...
|   |   |-- image
|   |   |   |-- ...
|   |   |-- text
|   |   |   |-- ...
|   |   |-- video
|   |       |-- ...
|   |-- index.js
|
|-- post.image-viewer
|   |-- components
|   |   |-- backdrop
|   |   |   |-- ...
|   |   |-- navigation
|   |   |   |-- ...
|   |   |-- viewer
|   |       |-- ...
|   index.js
|
|-- post@shared
|   |-- components#actions
|       |-- like
|       |   |-- ...
|       |-- reply
|       |   |-- ...
|       |-- repost
|       |   |-- ...
|       |-- share
|           |-- ...
|
|-- ...
```

Now we can access the resources of the `components#actions` group of the `post@shared` meta-category in both `post` and `post.image-viewer` categories.

Success! 🎉

#### Creating `PostEditor` and `TrendsWidget` components

For the `PostEditor` component, let's create the `post-editor` category.

The `PostEditor` has an emoji picker that will be used in several other places, so let's separate it and create the `emoji` category.

```
src
|-- emoji
|   |-- components
|   |   |-- picker
|   |       |-- ...
|   |-- values
|   |   |-- emoji-map.js
|   |-- picker.js
|
|-- post-editor
|   |-- components
|   |   |-- audience
|   |   |   |-- ...
|   |   |-- editor
|   |   |   |-- ...
|   |   |-- emoji
|   |   |   |-- ...
|   |   |-- media
|   |   |   |-- ...
|   |   |-- poll
|   |   |   |-- ...
|   |   |-- settings
|   |   |   |-- ...
|   |   |-- text-area
|   |       |-- ...
|   |-- values
|   |   |-- post-limits.js
|   |-- index.js
|
|-- ...
```

For the `TrendsWidget` component, we create the `trends` category.

```
src
|-- trends
|   |-- components
|   |   |-- trends-widget
|   |       |-- ...
|   |-- trends-widget.js
|-- ...
```

We are getting closer to the finish line! 🏁

#### Putting home page components together

The only thing left to do is to implement the home page and connect together the components that we implemented earlier.

To prepare components for display on the main page, they need to be decorated.

Let's create a `home` category. We create a `PostEditor` component in it. In the component code, import the `PostEditor` component from the `post-editor` category and add the necessary additional logic and styles.

Similarly, we implement other local components - `PostFeed` and `Trends`.

Now we combine the resulting components in the `Home` component.

```
src
|-- home
|   |-- components
|   |   |-- home
|   |   |   |-- ...
|   |   |-- post-editor
|   |   |   |-- ...
|   |   |-- post-feed
|   |   |   |-- ...
|   |   |-- trends
|   |       |-- ...
|   |-- index.js
|-- ...
```

After that, we will update the code of the root `App` component and integrate the `Home` component into it.

Now let's deploy the application to the stage and notify the QA engineer about it. After some time, he approved our changes.

The minimal implementation of the home page is finished! 🎉 

#### Be proud of yourself 🤗

Congratulations, you have learned **Virtual Nesting** by 90%. Structure your knowledge by learning the [methodology](#methodology) and start using **Virtual Nesting** in your projects! ✌️

### Project example

You can see an example of the project file structure at [CodeSandbox](https://codesandbox.io/s/virtual-nesting-react-v1-irwu2s).

⬆️ [Back to contents](#contents)

## Methodology

### Category

Category is a directory that is used to group [elements](#element) by belonging to **function**.

It creates a new namespace.

> ```
> # in the example below `layout` is a category
>
> src
> |-- layout
> |   |-- components
> |   |   |-- ...
> |   |-- helpers
> |   |   |-- ...
> |   |-- index.js
> |-- ...
> ```

#### Category location

Categories are located in the `src` directory and only in it.

#### Subcategories

Categories can have subcategories. A subcategory is a directory that is used to **decompose** complex functions.

For example, when one **complex** function breaks into several **simple** functions to make it easier to work with.

It creates a new namespace.

A subcategory differs from a regular category by having a **link** with parent categories (one or more) and an associated **[restriction](#category-restrictions)** on access to resources.

The link is expressed through **virtual nesting** — at the level of **directory name**, by listing the chain of names of parent categories separated by a dot.

> ```
> # in the example below `post.image-viewer` is a `post` subcategory
>
> src
> |-- post
> |   |-- components
> |   |   |-- ...
> |   |-- components#content
> |   |   |-- ...
> |   |-- index.js
> |
> |-- post.image-viewer
> |   |-- components
> |   |   |-- ...
> |   |-- index.js
> |
> |-- ...
> ```

#### Category restrictions

- External access (from other categories) to internal category resources is allowed *only* through modules **at the root** of the category.

  All resources that are planned to be used *from outside* (import in other categories) should be *re-exported* in these modules.

  This restriction provides **explicit** entry points to the category and provides **hiding** internal resources.

  Example:

  > <code>src/<b>post-editor</b>/components/emoji/component.jsx</code>:
  >
  > ```js
  > // ✅ it's possible
  >
  > import { Picker } from '../../../emoji';
  >
  > // ✅ it's possible
  >
  > import Picker from '../../../emoji/picker';
  >
  > // ❌ you can't do that, `post-editor` category does not have access to the internal resources of the `emoji` category
  >
  > import Picker from '../../../emoji/components/picker';
  > ```

- Subcategory resources can only be accessed from **parent categories** and **categories with a different name** (for example, from `app` to `post.image-viewer`). Access in **subcategories** to resources of **parent categories** is *prohibited*.

  This restriction structures **links** between categories and subcategories and avoids **cyclic dependencies**.

  > If resources *need* to be accessed simultaneously in the parent category and all of its subcategories, they must be placed in the [meta-category](#meta-category) @shared.

  Example:

  > <code>src/<b>post</b>/components/post/component.jsx</code>:
  >
  > ```js
  > // ✅ it's possible
  >
  > import ImageViewer from '../../../post.image-viewer';
  > ```
  >
  > <code>src/<b>post.image-viewer</b>/components/viewer/component.jsx</code>:
  >
  > ```js
  > // ❌ you can't do that, `post.image-viewer` subcategory does not have access to resources of parent category `post`
  >
  > import Like from '../../../post/like';
  >
  > // ✅ but it's possible
  >
  > import Like from '../../../home@shared/components#actions/like';
  > ```

#### Meta-categories

A meta-category differs from a regular category in that:

- access to meta-category resources is allowed in the **base category** (without meta-tag) and all its **subcategories** — and only in them,

- in the **meta-category** access to the resources of the **base category** and all of its **subcategories** is *denied**,

- a meta category can **override** regular category restrictions.

The name has the structure: `<base category name>@<meta tag>`.

> If the base category is not specified, access to the meta category is considered permitted *throughout the project*.

- **@shared**

  The @shared meta-category is used to store resources that need to be accessed *simultaneously* in the **parent category** and all of its **subcategories**.

  It overrides restrictions:

  - in the @shared meta-category, the restriction on direct access to internal resources **does not apply**, you can access resources *directly*
  <br />

  > Access to resources of the `@shared` meta category (without specifying a base category) is allowed throughout the project.

  Example:

  > <code>src/<b>post</b>/components/post/component.jsx</code>:
  >
  > ```js
  > // ✅ it's possible
  >
  > import Share from '../../../post@shared/components#actions/share';
  > ```
  >
  > <code>src/<b>home</b>/components/post-feed/component.jsx</code>:
  >
  > ```js
  > // ❌ you can't do that, the `home` category does not have access to the resources of the `post@shared` meta-category
  >
  > import Share from '../../../post@shared/components#actions/share';
  > ```

⬆️ [Back to contents](#contents)

### Group

Group is a directory that is used to group [elements](#element) by belonging to **type**.

It creates a new namespace.

> ```
> # in the example below `components` is a group
>
> src
> |-- layout
> |   |-- components
> |   |   |-- about
> |   |   |   |-- ...
> |   |   |-- layout
> |   |   |   |-- ...
> |   |   |-- logo
> |   |   |   |-- ...
> |   |   |-- navigation
> |   |   |   |-- ...
> |   |   |-- profile
> |   |       |-- ...
> |   |-- ...
> |-- ...
> ```

#### Group location

Groups are located within categories and only in them.

#### Subgroups

Groups can have subgroups. A subgroup is a directory that is used for **additional grouping** of group members.

It creates a new namespace.

The subgroup name has the structure: `<group name>#<subgroup name>`.

A subgroup differs from a regular group by having a **link** with parent groups (one or more) and an associated **[restriction](#group-restrictions)** on access to resources.

The link is expressed through **virtual nesting** - at the **name** directory level, by separating the group name and subgroup name through a hash, then by listing the chain of parent subgroup names (if any) separated by a dot.

> ```
> # in the example below
>
> # `components#content` - `components` subgroup
> # `components#content.text` - `components#content` subgroup
>
> src
> |-- post
> |   |-- components
> |   |   |-- ...
> |   |-- components#content
> |   |   |-- ...
> |   |-- components#content.text
> |   |   |-- ...
> |   |-- ...
> |-- ...
> ```

#### Group restrictions

- Access to subgroup resources is allowed only from **parent groups** and **groups with a different name** (for example, from `components` to `helpers#math`). Access in **subgroups** to resources of **parent groups** is *denied*.

  This restriction structures **links** between groups and subgroups and avoids **cyclic dependencies**.

  > If resources *need* to be accessed simultaneously in the parent group and all of its subgroups, you must place them in the [meta-group](#meta-groups) @shared.

  Example:

  > <code>src/post/<b>components</b>/post/component.js</code>:
  >
  > ```js
  > // ✅ it's possible
  >
  > import Content from '../components#content/content';
  > ```
  >
  > <code>src/post/<b>components#content</b>/text/component.js</code>:
  >
  > ```js
  > // ❌ you can't do that, the `components#content` subgroup does not have access to the resources of the parent group `components`
  >
  > import ProfilePopover from '../components/profile-popover';
  >
  > // ✅ but it's possible
  >
  > import ProfilePopover from '../components@shared/profile-popover';
  > ```

#### Meta-groups

A meta-group differs from a regular group in that:

- access to meta-group resources is allowed in the **base group** (without meta-tag) and all its **subgroups** — and only in them,

- in the **meta-group** access to the resources of the **base group** and all its **subgroups** is *denied*,

- a meta-group can **override** regular group restrictions.

The name has the structure: `<base group name>@<meta tag>`.

- **@shared**

  The @shared meta-group is used to store resources that need to be accessed *simultaneously* in the **parent group** and all of its **subgroups**.

  *It does not override restrictions*.

  Example:

  > <code>src/post/<b>helpers</b>/playback.js</code>:
  >
  > ```js
  > // ✅ it's possible
  >
  > import math from '../../helpers@shared/math';
  >
  > ```
  > <code>src/post/<b>components#content</b>/video/component.js</code>:
  >
  > ```js
  > // ❌ you can't do that, the `components#content` group does not have access to the resources of the `helpers@shared` meta-group
  >
  > import math from '../../helpers@shared/math';
  > ```

⬆️ [Back to contents](#contents)

### Element

Element is a module or directory with modules that export [resources](#resource).

> ```
> # in the example below `scroll.js` is an element
>
> src
> |-- layout
> |   |-- helpers
> |   |   |-- scroll.js
> |   |   |-- ...
> |   |-- ...
> |-- ...
> ```
>
> ```
> # in the example below `navigation` is an element
>
> src
> |-- layout
> |   |-- components
> |   |   |-- navigation
> |   |   |   |-- component.jsx
> |   |   |   |-- index.js
> |   |   |   |-- styles.js
> |   |   |-- ...
> |   |-- ...
> |-- ...
> ```

#### Element location

Elements are located within groups and only in them.

#### Module links within a directory element

Modules within a directory element can have links.

The link between the parent module and the child is expressed through **virtual nesting** - at the level of the module **name**, by *listing the chain of parent modules names separated by a dot*.

> ```
> in the example below, the `styles.actions.js` module is linked to the `styles.js` module
>
> src
> |-- post-editor
>     |-- components
>         |-- editor
>             |-- component.jsx
>             |-- index.js
>             |-- styles.js
>             |-- styles.actions.js
> ```

At the name level, we can implement a virtual directory:

> ```
> in the example below, the `assets` virtual directory is implemented
>
> src
> |-- post-editor
>     |-- components
>         |-- editor
>             |-- assets.emoji-icon.svg
>             |-- assets.media-icon.svg
>             |-- assets.poll-icon.svg
>             |-- component.jsx
>             |-- index.js
>             |-- styles.js
> ```

#### Element restrictions

- External access to the internal resources of a directory element is allowed *only* through the index module (`index.js`).

  All resources that are planned to be used *from outside* (imported in other elements, groups, categories) should be *re-exported* in the index module.

  This restriction provides an **explicit** entry point to the element and provides **hiding** internal resources.

  Example:

  > <code>src/post-editor/components/<b>editor</b>/component.jsx</code>:
  >
  > ```js
  > // ✅ it's possible
  >
  > import Media from '../media';
  >
  > // ❌ you can't do that, the `editor` element does not have access to the internal resources of the `media` element
  >
  > import mediaIcon from '../media/assets.media-icon.svg';
  > ```

⬆️ [Back to contents](#contents)

### Resource

A resource is any value exported from a module.

```jsx
// in the example below `Component`, `mathHelper` and `meaningOfLife` are resources

export const Component = () => {
  return <span>Hello, world!<span/>;
}

export const mathHelper = (r) => {
  return Math.PI * r**2;
};

export const meaningOfLife = () => {
  return wait('7.5m years').then(() => 42);
};
```

⬆️ [Back to contents](#contents)

### Compatibility with architectural patterns

Architectural patterns and file structure approaches share a common goal of making projects easier to work with.

If the architectural pattern and file structure approach are compatible, their combined use may be more effective in achieving this goal.

You can use **Virtual Nesting** in your project to further ease your work if the project architecture does not include file structure requirements or if these requirements are compatible with **Virtual Nesting** principles.

If the architecture requirements do not allow you to fully apply **Virtual Nesting**, you may also consider applying individual ideas of this approach.

⬆️ [Back to contents](#contents)

### General recommendations

- Avoid cyclic dependencies.
- Use kebab-case to name directories and files.

## Notes

> The word **function** appears frequently in the text. A function is a set of application properties that allows you to perform a specific task.

## FAQ

**Can Virtual Nesting be used with TypeScript?**

Yes!

**Can Virtual Nesting be used with CommonJS?**

Yes! Only the syntax of the modules will be different.

**Can Virtual Nesting be used with other languages?**

I guess so! If you succeed, please share your experience.

## Feedback

I appreciate any feedback from you! Please, feel free to leave a comment in discussions or issues.

## Contributors

Lead developer — [@austrokhart](https://github.com/austrokhart).

Ideas:

- [@daryabratova](https://github.com/daryabratova)
- [@a1ex-kaufmann](https://github.com/a1ex-kaufmann)
- [@kindaro](https://github.com/kindaro)

## License

(MIT License)

Copyright (c) 2023 austrokhart

This license permits persons who receive a copy of this software and accompanying documentation (hereinafter called “Software”) to use the Software free of charge without restrictions, including the unlimited right to use, copy, modify, merge, publish, distribute, sublicense and / or sell copies of the Software, as well as to persons to whom this Software is provided, subject to the following conditions:

The copyright notice above and these terms and conditions must be included in all copies or substantial parts of the Software.

THIS SOFTWARE IS PROVIDED "AS IT IS" WITHOUT WARRANTIES OF ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE, AND NON-INFRINGEMENT. UNDER NO CIRCUMSTANCES SHALL THE AUTHORS OR RIGHT HOLDERS BE LIABLE FOR ANY LAW CLAIMS, FOR DAMAGE OR OTHER REASONS, INCLUDING IN CONTRACT, DELICT OR OTHER SITUATION ARISING OUT OF THE USE OF THE SOFTWARE OR OTHER ACTIONS RELATED TO THE SOFTWARE.

## Support the author

I've spent hundreds of hours working on this project. I hope it saves thousands of hours of time for other developers while working on their projects.

If you want, you can support me on [Boosty](https://boosty.to/austrokhart).
