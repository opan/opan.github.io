---
layout: post
title: My first impression of using Hanami
categories: blogs programming
tags: ruby "ruby framework" "hanami framework"
---


Ever heard about [**Hanami**](http://hanamirb.org/) framework? If not, let me introduce you to this amazing framework. Hanami is formerly named as **Lotus**, [but for some reason](http://hanamirb.org/blog/2016/01/22/lotus-is-now-hanami.html) they changed the name into **Hanami**. Actually I'd prefer the new one, feels more Japanese to me (since the origin of Ruby languange is from Japan). Okay, let's go to the main topic. What we going to talk in this blog post is clearly described by the title of this blog post.

A couple months ago, I've got a task from my supervisor to explore new framework, and that's **Hanami**. At first glance, I just thinking it will be easy to learning this framework since they mention that **Hanami** is adopting MVC architecture just like Rails do. But after some exploration, I found that some parts of **Hanami** is really different with Rails. <!---We're going to discuss that difference.---> Let's discuss it one by one.

### Hanami Principles

**Hanami** adopting two principles, *Clean Architecture* and [*Monolith First*](https://martinfowler.com/bliki/MonolithFirst.html). In short, this means **Hanami** is already prepared from the beginning to be able transforming an Monolith Architecture to an Microservice Architecture. This is supported by **Hanami** directory structure. There is two main directory that **Hanami** provide to us when the first time we generate an **Hanami** apps with `hanami new apps_name` command and they are `apps` and `libs` directory.


```
.
|- apps
|   |- web
|       |- application.rb
|       |- assets
|       |   |- ..
|       |- config
|       |   |- ..
|       |- controllers
|       |   |- ..
|       |- templates
|       |   |- ..
|       |- views
|       |---|- ..
|- libs
|   |- your_hanami_apps
|   |   |- entities
|   |   |   |- ..
|   |   |- mailers
|   |   |   |- ..
|   |   |- repositories
|   |       |- ..
|   |- your_hanami_apps.rb
..
```

<!-- You can say that `apps` directory as a containers that hold all *micro* apps. As you can see in above tree struture, by default **Hanami** will generate a `web` directory for us. That's our **Hanami** apps name. This directory could be anything really. Here is the *Clean Architecture* relaying. -->

You can say that `apps` directory is as a containers that hold all our apps, while `web` is our app that created by default for us. You can add as many app as you want under `apps` directory and the name could be anything really. As you can see in tree structure above, you don't see the *model* part as in MVC under `apps` directory. That's because **Hanami** put the *model* parts under `libs` directory, which is
