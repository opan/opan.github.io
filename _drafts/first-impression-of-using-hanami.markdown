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

If you coming from Rails like me, you will notice from tree directory structure above that the `apps` is a bit different with Rails have. Yes, they don't have *model* part as in MCV inside `apps` directory. **Hanami** keep the *model* part under `libs` directory. This is what *Clean Architecture* in **Hanami** is. The reason they keep the *model* part under the `libs` directory is just because the *model* should be separated from the VC parts and of course, must be available all across apps inside the `apps` directory.


### The Model Part

**Hanami** used their own ORM, called *hanami-model*. This is built on top of [*rom-sql*](https://github.com/rom-rb/rom-sql) that also built on top of [*Sequel*](https://github.com/jeremyevans/sequel), a Ruby ORM just like *ActiveRecord*. But you can keep using *ActiveRecord* in **Hanami** if you want, they have flexibility for that.

*hanami-model* uses *Repository pattern*. You can see just from `libs` directory structure above, there is a directory named `entities` and `repositories`. Quoted from *hanami-model* repository;

> - Entity - A model domain object defined by its identity.
> - Repository - An object that mediates between the entities and the persistence layer.

When the first time I was trying to use *hanami-model*, it was a bit hard for me after the past two years I was pampered by the power of ActiveRecord. Well as you know, ActiveRecord give us a lot of methods that help us to do many SQL tasks more easier than when we using raw SQL. There is a lot of magic behind it. *hanami-model* only provides several methods to help us to do some basic CRUD SQL actions. With this, *hanami-model* contains less magic behind it. For example, do you ever use ActiveRecord method like `User.find_by_email("user@email.com")`? Did you find it in your code? Nope, this is because of ActiveRecord magic. But when using *hanami-model*, you must create it by your own. You'll exactly know what the method do, because you created it. I like this part of **Hanami**.


### The View part

You can see from tree structure above there is another directory beside `views`, there is `templates`. You can say that `views` directory is equal to `helpers` in Rails and the `templates`? This is where the HTML files exists, meanwhile Rails keep it inside `views` directory. Actually there is no big difference in this part. You can use `views` in **Hanami** to generate HTML parts same as `helpers` do in Rails.


### The Controller part

First, let's see what Rails default controllers looks like.

```ruby
# Normal controllers file path: app/controllers/sessions_controller.rb
class SessionsController < ApplicationController
  def new
    # Do something
  end
end
```

And then, let's see what **Hanami** controllers looks like.

```ruby
# Normal controllers file path: apps/web/controllers/sessions/new.rb
module Web::Controllers::Sessions
  class New
    include Web::Action

    def call(params)
      # Do something
    end
end
```

Did you see the difference? Yep, in Rails, the controller actions is keeped in one file named `SessionsController`, while **Hanami** treat a controller as a module only to wrap the controller actions. So if there is new action let say `create`, then you must create new file under `controllers/sessions` directory called `create.rb`. Easy to understand right? With this, you don't need to worry about the controllers getting fat, since they are already separated into each file for each controller actions. And also you may notice, there is no inheritance just like controller in Rails `SessionsController < ApplicationController`, instead **Hanami** use `include Web::Action` helper. It's pretty straightforward and again, no magic behind this. You'll exactly know where to find a method, because you included it by your self.

Each **Hanami** controller action should have one method called `call(params)` and the `params` as argument. You can named the argument anything really, but they name it `params` in **Hanami** documentation. Just like controller in Rails, you can pass instance variable from this action class to the *view* part.
