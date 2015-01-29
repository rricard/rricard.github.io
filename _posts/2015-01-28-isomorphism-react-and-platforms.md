---
layout: post
title:  "Isomorphism, React and client platforms"
date:   2015-01-28 18:34:00
categories: isomorphism react om cljs
---

This article could feel like I'm just writing on a trend. That's not true, I was thinking about it for a while now and the [React Native Announcement](https://www.youtube.com/watch?v=KVZ-P-ZI6W4) today just gave me a good excuse to write this !

There is already a batch of good posts about Isomorphism out there (such as [this one from Airbnb](http://nerds.airbnb.com/isomorphic-javascript-future-web-apps/) or [this one from a Mozillian](http://jlongster.com/Presenting-The-Most-Over-Engineered-Blog-Ever) where he explains how he implemented his isomorphic blog) so I won't spend much time explaining it. An isomorphic app uses the same code to render views on the server and the client. React makes it easier and better by being able to just change what was updated on the client side without re-computing the whole DOM. It is mostly costless to add the rendering on the server-side, React playing really well with Node. This is a good way to make mobile web apps start faster on bad cellular networks for instance.

But as far as we know, "mobile web apps" still have a pretty bad UX in general. The future is promising but we're not quite there yet unfortunately ! That's why Facebook started working on React Native (for iOS and Android and I hope they will be releasing it soon). As they said the whole thing is not about "Write once, run anywhere", it's about "Learn once, write anywhere". That doesn't mean that we can't share things between our modules.

But let's stop quoting other articles and let's explain what I have in mind.

## The isomorphic app, generalized

Here are some boxes and some arrows:

![App Architecture Overview](/public/images/isomorphic-overview.svg)

So, **what's new in this ?**

I'm a big fan of loosely coupled modules and that's what I want to develop here. A classic isomorphic app would only take the Frontend Server and client parts to render the DOM. The frontend server would directly query the database.

One thing that companies like Twitter are doing for a long time now is to separate frontend server code and the "logic", their backend. So I added this to my schema. You can see now that the Frontend server can be a generic brick binding the data from the API to the React views. That's exactly what I want to work on soon !

**So what's shared at this point ? What's reusable ?**

- The React components between the frontend server and the client
- The simple utility functions: pure, that can be shared by the backend and the frontend

**Wait... It's not loosely coupled anymore !**

It's still loosely coupled ! We just reuse some code but each module keeps it's concern and what it has to do:

- The backend provides an API (and that's even better if your backend is split in small modules interacting with each other)
- The frontend server just binds views and data (but for that, it needs to get access to the components !)
- The frontend client does the exact same thing but is able to handle user input at the same time

**Now, what about the Mobile Client ?**

That's the interesting part, but we don't know yet how it will be precisely implemented since Facebook has not released React Native for now. All we know is that it will bring the ability to write Native apps with JS and React. That's great news ! Writing apps in [JS on the native platform](http://www.appcelerator.com/titanium/) is not new. But JS alone doesn't make it easier to write applications. React does. That's why it's a great announcement.

That doesn't mean we'll be able to share the same components between the platforms but we can share the same logic between clients this time. (ie. the Flux pattern and how to interact with the backend...)

I don't know if this structure is the best way to do things for now, it has to be tested but it just feels natural to me.

## The implementation

**How can we do that ?** That's an interesting point !

We need a common language between all modules.

The natural response to this would be to use Node.js on every server obviously.

First of all, it's not mandatory to share code between the backend and the frontend so the backend could be written in whatever language you like (you don't have to change it at all !).

Here's what I would do **and on which I'm starting to work on**:

- The API backend would be running on the JVM written in Clojure.
- The Frontend server could be running on Node.js with the code written in Clojurescript using om. (Why not ? I'm working on a [ring-like API on top of core.async](https://github.com/rricard/ring-script))
- The Client frontend would be written in Clojurescript too and use om.
- **Same thing on mobile**: cljs+om ! Imagine having REPLs, figwheel and everything cool about clojurescript development while working on a native mobile app ! To achieve that we may need a little bit of work but that's the exciting part, isn't it ?
- When no side effects are needed, some functions can be shared between cljs and clj with [cljx](https://github.com/lynaghk/cljx).

With this stack, everything could be written in the same language, keep the performance of the JVM, keep the isomorphism with the Node runtime and share code between all the frontends while having different UI implementations.

Most of the required things to make it work are already there ! All we need is a little bit of magic glue and it's done !

I'll keep you updated on my research on this. If I find out it's not efficient or stupid, I'll tell you, don't worry ! I'll also show you some code if I have something cool working !

*If what I just wrote feels a bit fuzzy or if I made a major spelling mistake (that happens quite often), please open [an issue or a PR](https://github.com/rricard/rricard.github.io/issues) for this blog.*

**[Discussion on Hacker News](https://news.ycombinator.com)**
