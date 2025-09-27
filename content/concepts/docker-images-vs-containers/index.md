+++
title = 'A wall and a chalkboard: An Analogy to Understand Docker Images and Containers'
date = '2025-09-20T09:04:06+01:00'
draft = true
+++

# A wall and a chalkboard: An Analogy to Understand Docker Images and Containers

## Docker Image: The Permanent Wall

Think of a Docker Image as a finished, permanent wall. This wall is built from a stack of read-only layers, like transparent blocks fused together. Each block contains a specific piece of the application, like its operating system or other software dependencies. Because the wall is permanent and fused together, you cannot change any of its existing blocks.

Many software providers, like Ubuntu, offer official "walls" that you can use as a starting point.

## Container: The Chalkboard on the Wall

A Container is a live instance of an image. It’s what happens when you decide to *use* the wall. To make it usable, Docker mounts a **chalkboard** on top of the permanent wall.This chalkboard is a writable layer where all your changes, new files, and running processes happen. Every container gets its own separate chalkboard, so what you do on one doesn't affect another, even if they are mounted on the same wall. The wall itself is never copied; containers simply reference the existing wall, making them fast and efficient to create.

## Dockerfile: The Blueprint for the Wall

A `Dockerfile` is the builder's blueprint. It’s a simple text file with step-by-step instructions on how to build a wall. Each instruction, like installing software or copying application code, tells the builder to add another permanent, read-only block (a layer) to the stack.

## Custom Images: Reinforcing the Wall

Often, you don't build a wall from the ground up. Instead, you create a Custom Image. This involves taking an existing wall (like the official Ubuntu image) and giving the builder a blueprint that adds new, custom blocks on top. For example, you might add a block containing your own application's code. This is like reinforcing the existing wall or adding features to it, not building a completely separate new one.

## The `docker build` Command

This is the command you give to the builder. It reads your `Dockerfile` blueprint, gathers any necessary local files (the "build context"), and constructs your final wall (your custom image) exactly as you specified.

## The `docker run` Command: Putting It All Together

The `docker run` command is the action that mounts a fresh chalkboard onto a specified wall. This combination—the permanent wall (your image) plus the temporary chalkboard on top (the writable layer)—is what we call a **Container**.You can write, draw, and erase on the chalkboard all you want without ever leaving a mark on the underlying wall. When you are finished, you can either wipe the chalkboard clean and remove it, or leave it stopped on the wall to inspect later. You can even mount hundreds of separate chalkboards on the very same wall, and each will be a unique, isolated container.