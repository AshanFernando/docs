---
id: overview
title: Overview
---

Consider this project directory structure:

```sh
$ tree my-web-app
.
├── components
│   ├── app
│   ├── ui-primitives
│   │   ├── button
│   │   ├── table
│   │   ├── logo
│   │   └── title
│   └── pages
│       ├── homepage
│       └── about
├── readme.md
├── pnpm-lock.yaml
└── workspace.jsonc
```

No configuration or complicated folder structures, only a set of neatly organized components.

This is a **Bit Workspace**. It is focused on composing applications with components. We recommend breaking down your frontend application to its building blocks and composing pages, data-flows, forms, and applications using APIs. Components can be implemented in React, Angular, Vue, Stencil, and Node.

## Components as workspace modules

Each component in the workspace is managed as a module. While the component's source code is a part of your workspace, Bit keeps the compiled module in the `node_modules` directory.  
By treating each component as a module Bit helps you build isolated components that interact with each other using only their APIs.

```sh
./node_modules/@acme/button   -> ./components/ui-primitives/button
./node_modules/@acme/homepage -> ./components/pages/hompage
...
```

Each component should be required using an absolute `import` statement using its module name.

### Multiple types of module

Instead of using the same build configuration for all components in a project, when treating components as separate workspace modules you can have different builds for components. For example, a utility function can be built with a different configuration than a web component. It allows you to gradually refactor components from one technology to the other (transition to TypeScript, one component at a time). Learn more about it [here](/docs/environment/overview#how-environments-work).

## Initializing Workspace

Initialize Bit workspace by running `bit init` command. The folder in which the workspace was initialized, is set as the workspace root.

```sh
$ bit init
Initiazlied an empty Bit worksapce
```

### Integrate with existing project

Bit is designed to have a minimal footprint on your codebase. So you can embed a Bit workspace to any of your frontend projects. This way, you can improve your component workflow and publish individual components with minimum overhead.

Suppose you already have a project with components. In that case, you can initialize a Bit workspace in its root directory and use Bit to harvest pre-existing components using the `bit add` command. Once managed by Bit, you can use the `workspace.json` file to apply configurations for your components. The application itself can use the absolute import statements to use the modules Bit creates for each component in the project's `node_modules` directory.

### Create a new project

When utilizing component-driven development, entire applications can be considered as a composition of components. Instead of treating your entire project as an application, you can decide to develop a component to be an application and have that component composed of various other components in your workspace.

You can follow Bit's [getting-started](/docs/getting-started/quick-start) guide to experiment with such a workflow.

#### Starting with a project templates

> **Not yet implemented**
>
> project-template capability is not yet implemented.

## Workspace contents

When initializing a Bit workspace, you can manage individual components using a centralized workflow and configuration. Bit's footprint on a project is rather small, as it adds the following resources:

1. Workspace configuration - `workspace.json`
1. Component map - `.bitmap`
1. Workspace scope - `.git/bit` (or `.bit`).

### Workspace configuration

The `workspace.json` file is Bit's primary configuration file for a project. Use it to manage and maintain configuration for all components. Read more about it [here](/docs/component/component-json).

### Component map

Components are composed of files organized in a directory structure. Unlike other monorepo tools, Bit does not limit you to a predefined directory structure. You can create whatever directory structure you see fit.  
To keep track of the location of each component in your workspace, Bit uses the `.bitmap` file. Whenever a component is created, moved, or removed, Bit automatically updates this file.

### Local scope

The workspace's scope contains information about Bit components, such as source files, versions, and dependencies. By default, the components store is an extension to the git repository under `.git/bit` directory, but can be stored elsewhere, such as under a `.bit` folder.

> **Distributed storage**
>
> Bit scopes implement a distributed storage system, similar to Git. This means that all data stored locally is what gets pushed to the remote server. Read more about it [here](/docs/scope/overview).

## Bit workspace and Git

Make sure to track the following files with your SCM:

- `.bitmap`
- `worksapce.json`

You should not track the workspace scope with Git.