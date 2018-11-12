---
layout: post
title:  "Building a Better Transit App - Part 1"
date:   2017-02-05 15:23:35 -0700
categories: transit, tech
image: "/images/blog/bus-stop.jpg"
---

I have spent a lot of time recently learning and working with Xamarin Studio and I wanted a useful project to build to exercise my new skills.  Living in PDX I use public transportation a lot, almost every day in fact.  I am either on my bike or on the bus to and from work and most errands around town.  I am constantly frustrated by the inaccuracy of Google Maps’ transit information and the poor usability of other apps, so I am setting out to fix that.  I am building this app for myself more than anything, but I will document my process here and release the code to GitHub as I go.

This post serves as my scope for the project, and a place to document my goals, requirements etc.  I have already built a prototype that my wife and I have been using around town, and we both like it but it needs refinement and simplification.  For phase one of this application, my goal is to provide simple up-to-the-minute times for nearby transit stops.  This helps solve my main problem of planning when to leave to catch a bus or streetcar to get somewhere, I already know how I am going to get there the question is when.

<img src="{{ site.baseurl }}/images/blog/bus-stop.jpg" alt="commuter train">

*Phase 2* will add exploration to the application.  Sometimes you need to know where a bus goes, what are the stops for that bus, etc.  With this, I want to give myself the ability to look for alternate routes that happen to be going in the direction I want to go or discover a bus route by just knowing the bus number from the stop I’m standing at.

With that, here are some more specific requirements, technologies, etc:

* iOS app using Xamarin Studio
* Leave opportunity to add Andriod using Xamarin Studio for later
* Leveraging Esri mapping technology
* Separation of “logic” and “GUI” using ViewModels
* Leverage functional programming, in this case, use classes for data/models and functions for actions (where appropriate)
* Experiment more with event-based programming
* Include tests at the function, model, and ViewModel level

I’ll be posting the code to GitHub in my [Public Transit App](https://github.com/MoravecLabs/PublicTransitApp) repository while working on this project.

I think that wraps up the introduction/plan for this little project!  Off to write code!