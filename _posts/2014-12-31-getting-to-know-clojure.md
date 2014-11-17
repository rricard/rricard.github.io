---
layout: post
title:  "Getting to know Clojure"
date:   2014-12-31 16:51:00
categories: clojure
---

> ⌛ **This article is quite old!** This article dates back from when I was still studying computer science in university. I was back then quite enthusiastic about a lot of things and this article reflects that well! It may be out of date with my current way of thinking and working as I discovered different constraints in software development. Just keep that in mind and enjoy!

Hi hello ! First of all let me introduce all of this: this is a blog, yeah. This is exactly my third one, the other ones are cut off the Internet for the following reason: I was ashamed of them. Didn't talked that much about things that I like but stuff I thought it was good to have in a blog. As a result, it was not good material.

So this one is my third and last take at a blog. If this one fail, I'm not tring again ! So I'm only going to speak about what I want to speak about !

So I want to speak about clojure today ! 2009 was my year of PHP (a dark year). 2010 was the year of the Python enlightenment. 2011 I discovered Rails and then Ruby. 2012 was the year of the webdev with the killer couple: Rails & jQuery. 2013 was the year of JS as a go-to everything language with Node.js. 2014 was the year of doubts, I tried many things such as functionnal programming in JS or Python, golang, struggling with Java (had to: Android) and many other stuff. Always feeling not good about a few points when experimenting and toying with those (except Java where everything was quite wrong). I knew clojure was really loved by it's community and was quite curious about this. But didn't had much time to learn it. So yeah, I decided it: 2015 would be the year of clojure for me !

**TL;DR** Clojure is awesome, you should definitely take a few days (5/6 days) to learn it. That's enough because it's awesomely minimal too !

## The path to enlightenment

First things first, I won't explain again why clojure is awesome. Search on Google or HN, there are many articles saying that clojure is awesome and I personally never saw anyone complaining about it online. I don't see often languages that people don't complain about. One reason could be that LISPs never made it to the mainstream but I'm quite sure that there are really few points on which clojure can be criticized on (except from being a LISP, which in my opinion is a good thing).

The only thing I will say here is: Clojure is insanely simple to learn ! Not easy, **simple**. And I would like to introduce to you how you can learn it too. And get to like it !

### Day 1: Struggle

Clojure is not something you know right there, out of the box. It's not like picking up another imperative/OO language and start a project just like that. Nope you'll need to get used to it. I never said it is easy but you'll see the reward later. Everything becomes effectively easy then.

First of all, and it is essential, you must get the right tooling before even starting: the `lein repl` is ok but you better get the repl into your text editor. Often clojurists use emacs. Don't go away ! I personally don't like using emacs. So like other clojurists out there I use [vim-fireplace](https://github.com/tpope/vim-fireplace) but if you're more of an IDE person, just use [counterclockwise](https://code.google.com/p/counterclockwise/) or [cursive](https://cursiveclojure.com/). As long as you can fire repl commands from your editor, it's okay !

At this point I recommend you to start with a brief tutorial even if you don't understand everything there. It's better than diving directly into a book. I recommend the excellent [Clojure Doc's Introduction](http://clojure-doc.org/articles/tutorials/introduction.html) for that.

Now you can rest or do something else ! Let your brain go past the first confusing things you encountered.

### Day 2: Understand

Ok, now you must be curious. Manipulating immutable data must feel weird for now. That's ok, you'll get used to it. At this point you should print a [cheat sheet](http://clojure.org/cheatsheet), that'll help later !

Right now you don't really know what clojure is all about. That's why you should directly try to do fun stuff with it instead of diving into macros or other stuff.

Just try to mount a simple [ring server](https://github.com/ring-clojure/ring) that writes back the path you inputted in your browser. After that, try some routing with [compojure](https://github.com/weavejester/compojure). Without really knowing it, you already made an highly concurrent webserver. Then you can easily play with some templating engines or try to connect to an SQL database with jdbc. One other thing to do is to try the java interop by creating a frame and drawing inside it from the repl. Take a look at the [toolbox](http://www.clojure-toolbox.com/) to know what the community already did for you !

I won't give you more clues, you have to learn how to find things by yourself but don't worry there's a lot of material available online.

### Days 3-4: Start a project

At this point you should start writing something, a project. Cool or not, already existing or not... It doesn't matter ! Do whatever you want even if it's reinventing the wheel ! I personally implemented a basic [Paxos consensus system](https://github.com/rricard/consens) (I like networking stuff !). But do whatever you like !

It's ok if you don't know everything. You'll figure out later that if you can do something with basic clojure, you better stick with it ! Only use complex stuff when needed ! (In case of interop or in desperate need for a state). But try to stick with the most basic clojure you know when possible (the more you rely on pure functions, the best it is).

Last thing here: write your project by using intensively tests and the repl. They are your best tools. If you don't, clojure will feel painful to you !

### Days 5-6: Continue your project and start reading a book

Everything's in the title ! I'm just there and I feel great right now, I learn new things but I know how to do many things, and it only took me a small week. Reading books and continuing to learn is important. I may have understood 90% of the language, the essentials, but the last 10% may take much time (as they are not essential in every project).

If you want a word of advice, _[The Joy of Clojure](http://www.joyofclojure.com/)_ is a great book to work with. It won't spend much time learning you the basics of clojure (you already did !) and will bring you directly to the new parts.

## Have fun !

Really, if you start not liking clojure after the rough first days, you might not be in the right place and that's ok. It must feel fun, because it is. It must feel predictable, because it is. And at the end you'll start doing less and less mistakes, the repl being your strongest ally there !

I hope I motivated you learning clojure ! If not, try golang like everyone else ! (Without kidding, golang must be an awesome language, it just never worked really well with me.).

Anyway, be prepared to hear more about clojure soon I hope !
