---
layout: post
title:  "Building a Better Transit App - Part 2"
date:   2017-02-14 5:33:32 -0700
categories: transit, tech
image: "/images/blog/transit.jpg"
---

For the first bit of code, I set out to get a working map and a button to press to zoom the map to my current location. I ended up spending a lot of time cleaning some of the code that I brought over from other projects, especially once I introduced [StyleCop]("http://addins.monodevelop.com/Project/Index/54") to my workflow. That took some getting used to, but now that I am starting to get the hang of it, I like it. Adding StyleCop forced me to be more honest about the organization of my code, hopefully making it better code.

With that done, and my code cleaned up it was time to Add the Esri SDK and Put a map on it! I ended up adding a lot of complexity to such a simple task, but this complexity should make my life much easier as the app grows, and it certainly makes the view controller pretty straight forward.

<img src="/images/blog/esri-ss-1.png" alt="screen shot">

```
public override void ViewDidLoad()
{
    base.ViewDidLoad();

    // Create any objects and binding events.
    this.viewModel = new MainMapViewModel();
    this.viewModel.Map.SubscribePropertyChanged((oldVal, newVal) =>
    {
        this.MapView.Map = newVal;
    });

    // Initialize objects.
    this.viewModel.Initialize();
}
```

This code makes use of my Atom class and a general pattern I am trying out where values are not set in the constructor of the ViewModel, instead, they are set in either Initialize or InitializeAsync depending on what my needs are.  Using the SubscribePropertyChanged method on the Atom wrapper of the Map I can put an anonymous action that always resets the MapView.Map whenever the viewModel.Map changes.  This works much the same way a one-way binding does in WPF only here I am doing it all in code.  Using an anonymous function like this provides a lot of power, essentially allowing me to handle any conversions needed in addition to listening to the event.  In a way, it is like combing the binding and value converter concepts from WPF into a single function.

This makes my ViewModel look like this:

```
public class MainMapViewModel : BaseViewModel
{
    /// <summary>
    /// Initializes a new instance of the <see cref="T:PublicTransit.MainMapViewModel"/> class.
    /// </summary>

    public MainMapViewModel()
    {
        this.Map = new Atom<Map>(null, this, "Map");
    }

    /// <summary>
    /// Gets the map.
    /// </summary>

    /// <value>The map.</value>
    public Atom<Map> Map { get; private set; }

    /// <summary>
    /// Initialize this instance.
    /// </summary>

    public void Initialize()
    {
        this.Map.Value = MapUtilities.CreateStandardMap();
    }
}
```

In this case, the ViewModel contains a Property Map which is an Atom wrapper of an Esri Map object.  It gets an initial creation in the constructor but does not get a real value until the Initialize function is called.  This allows me to set up my events (binding) before the value is set.  You can’t always set the initial value to null like I have here.  If the Atom is wrapping a not nullable type like int or double you will have to set a value, but as long as the value changes in Initialize, the change events will fire.

You may also notice that I am using a static function to generate the actual map.  I did this because I am anticipating multiple maps that will have the same or very similar data and want a central place to create them.

### But Location…

I still needed to zoom the map and get a button there so that I could zoom to my current location.  Setting the initial zoom of the map was pretty easy and I settled with this to start:

```
map.InitialViewpoint = new Viewpoint(45.522689, -122.678187, 4000);
```

Now I needed to add a button to the map so that I could zoom to my location and get nearby stops. My first attempt was not so cool:

<img src="/images/blog/esri-ss-2.png" alt="screen shot">

But after adding in a Material Design library I was able to get something much nicer:

<img src="/images/blog/esri-ss-3.png" alt="screen shot">


That’s about it for this installment, next I plan on getting the actual stop locations into the map.  I added a few extra details below for those interested.  You can find this [Public Transit App code]("https://github.com/MoravecLabs/PublicTransitApp/tree/Part-2") on GitHub.

### A few details that I didn’t include within the main blog:

**Atom** – A small class that acts as a wrapper for any type. Once wrapped the data is set or access via the Value field. In addition, it can automatically be registered with a view model so that NotifyPropertyChanged works correctly. An Atom also has its own event subscription so that you can simply subscribe to a change. The idea is to give me a very event-driven tool that I can use within any framework. One of the issues I found while building the prototype of this app was the complexity in connecting ViewModels to Controllers in iOS when the two concepts work a bit differently. I found myself writing a lot of code, using Atoms I can reduce that code by creating “bindings” by using events.

**StyleCop** – I want this app to be readable so that I can refer back to it in the future. StyleCop seems to be a good way to keep everything formatted well. In Python, I always try to follow PEP-8 as best as I can, but I didn’t know what to use for C# until my wife (@codergrl on [GitHub]("https://github.com/codergrl") and [Twitter]("https://twitter.com/codergrl")), told me about StyleCop!

**MaterialDesign** – I forgot this on my main list last week, but I do want this app to be nice to look at so I’m going to try out some of the Xamarin iOS material design plugins to see what happens.  I ended up using [Material Design for iOS by FPT]("https://github.com/fpt-software/Material-Controls-For-iOS").