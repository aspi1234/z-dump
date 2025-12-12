+++
title = "Abstraction might be the only REAL barrier between YOU and YOUR full potential"
date = '2025-12-12T05:37:46+01:00'
draft = false
series = ["Fundamentals"]
series_order = 1
showWordCount= true
showDate = true
enableCodeCopy = true
heroStyle= "background"
+++

{{< figure
src="featured.png"
nozoom=true
alt="Abstract Computer Image">}}

**TL;DR :** As an Information Technology practitioner, you deal with **"Abstraction"** every single day. It is the tool that makes our lifes easy. It allows us to use _APIs, SDKs, and drivers_ without worrying about the billions of `0s` and `1s` flowing through the circuitry.<br>

But this simplicity is a trap. **Abstraction** is a _double-edged sword_. While it helps beginners start quickly, it often hides the foundational knowledge that you need to solve complex problems. It creates a **"comfort layer"** that feels _safe_ but _limits_ the ability to think deeply.

Think of it like _driving a car_. To drive to the grocery store, you only need to know the _abstraction_: the steering wheel, the pedals, and the key. You don't need to know how the fuel injectors firing into the combustion chamber work.
But what happens when the car _breaks down_ in the middle of nowhere? Or when you want to customize the engine for better performance? The **"driver"** is _helpless_. Only the person who has looked under the hood—_crossed the abstraction layer_—can fix it.
In this post, we will explore why crossing this layer is not just an academic exercise, but a mandatory step for unlocking your full engineering potential regardless of the field.

---

**Abstraction** is a concept or paradigm that helps implement or easily explain difficult concept without going deep into the how and why things work the way they do. Even if you aren't a CS student you must have surely come accross the sentence : "computers are made of 0s and 1s" or better yet " all computers understand are `'0s'` and `'1s'`.These `'0s'` and `'1s'` referred to as _bits_, are the building block of how _information_ _( images, music, documents and everything in between)_ is being _created,stored, modified, presented_ and _transmitted_ accross devices. These **bits** are actually an _abstractions_ of the state of current at a given point in time that is traversing a particular electronic circuit that makes up our devices (computer, phone etc...). This concept was used in the early days of computers through punching cards as shown in the following video by [core dumped](https://www.youtube.com/watch?v=qgwrt7vYY4U). But thanks to _abstraction_, you rarely have to think about them. You don't manage the raw state of electricity; you manage _files, windows, and variables_.

While abstraction allows beginners to build amazing things quickly, it creates a **"Comfort Layer"** that creates a _false sense_ of mastery. It hides the _HOW_ and _WHY_ things work.

Let's make this more vivid with the folllowing _car analogy_ : imagine learning to drive a car. In the beginning, you don't need to know how the _fuel injectors_ firing into the _combustion chamber_ work, or how the _CAN system_ transmits data. You just need the abstract interface:

- Insert the key into the key slut.
- Use the steering.
- Press the pedal.
- Follow the rules of the road and you are good to go.

The _steering wheel, gear box, pedals etc..._ are **abstractions** of the complex machinery under the hood. They are enough if you just want to commute. But what happens when the car breaks down in the middle of nowhere? Or when you want to customize the engine for better performance ? You are stuck. The "driver's" knowledge is useless To _evolve_, you **MUST cross the abstraction layer**.

Here are some examples of abstraction you come accross everyday and how crossing the abstraction layer might help you :

**1. Filesystems an abstraction of the raw storage sectors on physical disks**

> - The Abstraction: You see folders, documents, and a clean hierarchy. You save a file, and it sits there. Simple.
> - The Reality: Underneath, the filesystem is a complex translator managing raw storage sectors on a physical disk (HDD or SSD). It deals with _inodes, block allocation, journaling_ to prevent corruption, and physical wear-leveling.
> - Why You Must Cross It: If you never look deeper, you won't understand why a database is slow, why a file is "corrupted," or how to recover data that was "deleted." You are trusting the abstraction to never fail—until it does.

**2. Processes as an abstraction of the CPU,RAM and storage**

> - The Abstraction: You double-click an app, and it runs. You see a window open and stay open. It feels like a dedicated continuous flow.
> - The Reality: The CPU, RAM, and Storage are frantically juggling resources. The Operating System is performing "context switching"—_freezing one program, saving its state, running another for milliseconds, and switching back—thousands of times per second_. It is a chaotic dance of scheduling and memory management.
> - Why You Must Cross It: When your application hangs or your server crashes under load, the "double-click" knowledge won't save you. You need to understand _threads, memory leaks, and race conditions_ the mechanics of the process abstraction.

**3. Encoding: an abstraction of Language**

> - The Abstraction: You type "Hello" or paste an emoji. You see human-readable characters.
> - The Reality: The computer has no concept of "A," "B," or "😊." It only knows numerical values mapped to shapes via standards like ASCII or UTF-8. It is shifting bytes, handling big-endian vs. little-endian architecture, and interpreting hex codes.
> - Why You Must Cross It: Have you ever opened a file and seen a bunch of nonsense symbols (??) ? That is a broken abstraction. If you don't understand how data is represented (encoded) between programs and humans, you cannot fix communication errors between systems.

{{<alert>}}
**Disclaimer : Crossing the abstraction layer isn't easy. It requires you to leave the safety of the "driver's seat" and get your hands dirty with the engine. It is a process that is simultaneously painful and liberating.**
{{</alert>}}

**But the reward is worth the struggle**.

When you peel back the layers of _filesystems, processes, and encoding_, you aren't just learning trivia. You are updating your mental model. You are expanding your ability to _think, debug, and innovate_ in ways that surface level knowledge simply cannot support.

So, here is my challenge to you: **Don't just drive. Become the driver and the technician**

Next time you encounter _a bug, an error code, or a tool that works "like magic,"_ don't settle for the **easy fix**. Dig deeper. Break the **~comfort layer~**. Because that hidden layer contains the _foundational knowledge_ that turns a _practitioner_ into a _master_.

Until then **Happy learning**, and see you under the hood.

---

{{< alert "twitter" >}}
**Aside** : I am currently working on sharpening my technical writing skills through a specialized course, and this article is part of that journey.

- If you found this useful: Kindly drop a like it tells me I'm on the right track.
- If you didn't: Please comment with what didn't work for you. Honest feedback is the only way I can cross my own "abstraction layer" of writing and improve.

Also I am currently crossing the comfort layer of `Unix system programming` feel free to share with me any good ressources I can use to make this journey a reality
{{</alert>}}

**Thank you for reading!**
