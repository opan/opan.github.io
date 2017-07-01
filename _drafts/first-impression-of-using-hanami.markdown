---
layout: post
title: My first impression of using Hanami
categories: blogs programming
tags: ruby "ruby framework" "hanami framework"
---

Ever heard about [**Hanami**](http://hanamirb.org/) framework? If not, let me introduce you to this amazing framework. **Hanami** is formerly named as **Lotus**, [but for some reason](http://hanamirb.org/blog/2016/01/22/lotus-is-now-hanami.html) they changed the name into **Hanami**. Actually I'd prefer the new one, feels more Japanese to me (since the origin of Ruby languange is from Japan). Okay, let's go to the main topic. What we going to talk in this blog post is clearly described by the title of this blog post.

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

You can say that `apps` directory as a containers that hold all our apps, while `web` is our app that created by default for us. You can add as many app as you want under `apps` directory and the name could be anything really, for example you want add *Admin* feature then you can add `admin` app.

If you coming from Rails like me, you will notice from tree directory structure above that the `apps` is a bit different with Rails have. Yes, they don't have *model* part as in MCV. **Hanami** keep *model* part under `libs` directory. This is what *Clean Architecture* in **Hanami** is. The reason they keep the *model* part under the `libs` directory is just because the *model* should be available and shared all across our apps that live under `apps` directory.


### The Model Part

**Hanami** used their own ORM, called *hanami-model*. This is built on top of *[Sequel](https://github.com/jeremyevans/sequel)*, a Ruby ORM just like *ActiveRecord*. But you can keep using *ActiveRecord* in **Hanami** if you want, they have flexibility for that.

*hanami-model* use *Repository pattern*. You can see clearly from tree directories struture above, there is a directory named `entities` and `repositories`. Quoted from *hanami-model* repository;

> - Entiry - A model domain object defined by its identity.
> - Repository - An object that mediates between the entities and the persistence layer.
