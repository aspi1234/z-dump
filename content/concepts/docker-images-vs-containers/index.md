+++
title = 'You genuinely understand Docker images and containers if you have already used a chalk [white] board'
date = '2025-09-20T09:04:06+01:00'
draft = false
showWordCount= true
enableCodeCopy = true
showDate = true
heroStyle= "background"
+++

{{< figure
src="featured.png"
nozoom=true
alt="Abstract purple artwork">}}

# A wall and a chalkboard: An Analogy to Understand Docker Images and Containers

## `Docker Image`: The Permanent Wall

Think of a `Docker Image` as a **finished**, **permanent** _"wall"_.

This **wall** _(image)_ is built from a stack of **read-only layers**, like transparent blocks stacked on each other.

Each _block_ contains a specific piece of the application, like its _operating system_ or other _software dependencies ( text editors, browsers, etc)_.

Because the wall is permanent and the blocks fused together, you cannot **change** any of its existing blocks.

Many software providers, like Ubuntu, offer an official "foundation wall" (base images) version of their software that can be used to jumpstart a project.

## `Container`: The Chalkboard on the Wall

A `Container` is a live instance of an image.

It’s what happens when you decide to use the wall.

To make it _usable_, Docker **hangs** a _writable_ _chalkboard_ over the the permanent wall. This **chalkboard** is a **writable** layer where all your _changes_, _new files_, and r*unning processes* happen.

Every **container** gets its own separate _chalkboard_, so what you do on one doesn't affect another, even if they are mounted on the same wall.

The wall itself is never **copied**; _containers_ simply reference the existing wall, making them fast and efficient to create.

## `Dockerfile`: The Blueprint for the Wall

A `Dockerfile` is the builder's _blueprint_. It’s a simple text file with step-by-step instructions on how to build a wall.

Each instruction, like **installing** software or **copying** application code, tells the builder to add another permanent, read-only block (layer) to the stack.

## `Custom Images`: Customizing the Wall

Often, you don't build a wall from the ground up.

Instead, you create a `Custom Image` from an _existing_ image. This involves taking an existing wall (like the official Ubuntu image) and giving the builder a blueprint that adds **new**, **custom** blocks on top.

Think of it as adding a custom coat of paint or a detailed mural (your application code) on top of the existing structure. You're creating a new, unique design without rebuilding the entire wall.

## The `docker build` Command

This is the command you give to the builder.

- It reads your Dockerfile blueprint,

- gathers any necessary local files (the "build context")

- and constructs your final wall (your custom image) exactly as you specified

## The `docker run` Command: Putting It All Together

The `docker run` command is the **action** that mounts a fresh _chalkboard_ onto a specified wall.

This combination—_the permanent wall (your image)_ plus the _temporary chalkboard on top (the writable layer)_—is what we call a `Container`.

You can _write_, _draw_, and _erase_ on the **chalkboard** all you want without ever leaving a _mark_ on the underlying wall.

When you are done, you can either wipe the chalkboard clean and remove it, or leave it stopped on the wall to inspect later. You can mount hundreds of separate chalkboards on the very same wall, and each will be a unique, isolated container.

But remember, each of these containers occupies disk space, so remember to clean them up.

---

Thanks for reading. Until next Time, cheese :)

This post is a result of the frustration I got when I needed to create a 2gb Linux VM to clear some doubts I had regarding permissions on Linux (turns out this resulted in another post [^1]).

[^1]: A post on [Linux permissions]({{% ref "sudo-vs-setuid" %}})
