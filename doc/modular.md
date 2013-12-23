
# Building modular applications with Riot.js

[TOC]

# What is Riot.js?

## 1. A minimal approach to MV* world
Riot is a client-side MVP framework to build modular single page applications. It weights 1kb and has 3 public methods so it's extremely simple and easy to learn. You'll be in complete control and there is no extra stuff on your way. You start small and add stuff as you need it. Not the other way around. Minimal approach helps everyone to understand the pieces that make your application.

## 2. Vanilla JavaScript and jQuery
Riot uses vanilla JavaScript to structure the code and jQuery to build the user interaction. You'll master classic design patterns and elementary JavaScript instead of framework specific idioms. Frameworks come and go but universal programming skills are forever.

## 3. Modular code
The purpose of Riot is to build modular applications that are easy to manage and extend by multiple developers.  Your application will be "loosely coupled". Riot.js is all about modularity and the documentation centers solely on this purpose.

## 4. API oriented
The "frameworkless" nature of Riot forces you to focus on designing the API instead of building things around a certain framework. Your precious business logic is pure JavaScript that runs on server too.

## 5. Demo application
Riot.js comes with an example application that goes beyond a Todo MVC. It's an administration panel that is easy to continue with. Something useful. It's well documented and shows the basics of modular programming and API centered design.

This documentation is split into two parts: 1) outlining the core concepts in modular single page applications and 2) code samples. After those you can peek the source code of the [demo application](https://github.com/moot/riotjs-admin) to get full understanding of modular Riot applications.


# Modular applications

Your job is to build applications that are split into modules. Each module performs a logically discrete function and it's not dependent on other modules. This has major benefits:

1) A large program can be broken into smaller and simpler units
2) Modules can be added/removed/modified without breaking the application
3) Several programmers can work on individual modules at the same time
4) The program structure is easy to understand even for newcomers
5) Ability to build a different subset of modules for different needs

Modularity is the single most important key for large scale applications.

## What is modular?

A modular application consists of two things:

1. Core
2. Modules

The application core is a piece of software with a well documented API. It's not just a data layer, that you might have seen on other MV* setups. It's your precious business logic.

The modules are extensions to the core. These modules are "loosely coupled" meaning that they hook to the application core by "listening" to it. The core application is not aware of these modules.

There are no hard-coded function references. Strong coupling does not scale. There can be hard-coded dependencies inside the core, but not between the core and modules.

To actually keep things modular is constant organization work. Things should be on their most logical place. Think layers in Photoshop, table of contents on a book or visual hierarchy in an user interface. It's constant balancing. And when you add features it must fit to the big picture.



# Model, View and Presenter

How to build modular single-page applications?

Let's clean the table.

First you need to know the goals of your application. What id does and what it doesn't do? You model your business with JavaScript. That's the application core. Also called as Model.

Then you have the browser and the user interface. The layout (HTML), document object model (DOM) and the style (CSS). The piece that user sees and interacts with. That's the View.

Then you need to bind these two things together. You need to listen to what happens on the View: clicks, keypresses and scrolls. You also need to listen to the Model: maybe a new entry came trough a real-time channel or somone liked a post. This someone listens to all these events and reacts accordingly.

This "middleman" is called the Presenter.

Riot uses these terms to describe the big picture: Model, View and Presenter.

MVP at it's heart aims to separate the application logic (Model) from the user interface (View). This separation is important because it simplifies your code and makes it more testable. The lack of this high level separation causes so called "spaghetti code" where business and UI logic is mixed together. This is the jQuery era before single page applications (SPA) started gaining popularity.

In classic UI terminology Riot uses the the "passive view" branch of the MVP family.

> A Passive View handles reduces the behavior of the UI components to the absolute minimum by using a controller that not just handles responses to user events, but also does all the updating of the view.

Unlike most MVC-style configurations, Passive View results in no dependencies between view and model. It's a simple pattern and easy to understand – which makes you powerful.

Lastly, it's important to realize that MVP (or MVC) is not a framework. It's a high level design pattern. It's purpose is to simplify the architecture of UI heavy applications. A way to structure your code. In loosely coupled application the modules communicate with each other with events.

Most of the current frameworks are an overkill since basically all you need is a proper event system.



# Model

Model is the application core. It's the most important part on your application since everything is build on top of it. It's the public interface to the rest of the world. You'll be using it, your team will be using it and 3rd parties are using it.

A well designed application core can be extended with loosely coupled modules and allows anyone to develop the system on their own.

In Riot the model is a complete application and not just a helper object for the presenter layer as you might have seen on MVC configurations. It's also a good practice to keep your Models fat since the closer you come to the view the harder it becomes to test the assets. Anything that is difficult to test should have minimal behavior.


This allows multiple different user interfaces to be build on top of the same Model. Think different Twitter client's for example (when things were fine with them). People could develop wildly different experiences on top of the same API without knowing about each other.


## Designing the Model

Model is the starting point of the application design. You should reserve time for designing the Model and make it as simple as possible because you'll be spending lot of time with it later. Set the bar high. Think jQuery API.

Two things to keep in mind:

1. Which problem the product solves?
2. Who is using the product?

Model is a domain specific thing. Think what your application does, what are the goals, what features are there and what features are *not* there. Breaking your business into pieces / logical modules and thinking how they communicate. Something you may have done with object oriented languages in the past.

Keep these in mind when designing the properties, methods and events. Not going to go deeper here, but the API is the root of all good or evil.



## Backend

Riot does not include a separate backend component and this is purposely left open. Currently REST dominates the way of thinking but Web Sockets and real- time patterns are just around the corner, where RPC pattern makes more sense.

Your backend interface can just have a generic `call` method and the underlying implementation can be changed. Be it REST, RPC or a custom AJAX based thing.



# View

The view is what user sees and interacts with. The HTML page on a web browser. What's actually interesting for a JavaScript developer is the document object model (DOM). You can do all kinds of creative tricks to build user experiences. Most importantly it's a source of events:

1) User events: click, scroll, keypress, mousemove etc..
2) Document ready event
3) URL change events

These events are in special interest for Presenters, which perform the actual manipulation of the view. The view itself has no logic – just plain old HTML and CSS code.


# Presenter

Presenter is the important middleman between the View and Model. Each presenter is a loosely-coupled module performing a discrete function and can be developed individually.

Presenter listens to the View events and calls the appropriate methods on the API. It also listens to the Model events and manipulates the DOM accordingly.

Presenter doesn't make any assertions about the business model.

Note that other frameworks may call call this layer a "view", but in MVP this is called the "presenter".


## Templating

I like to start developing a single-page application with just HTML and CSS only. It's amazing how much you can do with those. You can fine tune the design and user interface to almost complete state. By adding simple CSS class name switches with JavaScript you can even complete those beautifully animated view switches. In reality, of course you cannot keep your hands out of JavaScript but it's totally possible to show the customer a complete application UI without single line of the actual logic.

What's best is that you can naturally continue from that HTML view to complete the whole application. In ideal world you even have the business logic completed and the API hasn't had any big changes for a while. Only presenters are left to hook these two together.

This HTML mockup is your collection of templates. Before starting with the presenters you can move some of the HTML inside `<template/>` tags, typically the ones that are rendered multiple times inside loops.


### No logic

Riot takes a strong position on not recommending logic inside the templates. There are multiple reasons for this:

1. Pure HTML is cleaner and passes W3C validator. A HTML mixed with a template syntax is messy.

1. HTML is not inherently meant to describe logic

1. Template loops are unnecessary on realtime applications where the iteratable lists change over time

1. Logic inside HTML is hard or impossible to test

1. Logicless templates are sometimes over a magnitude faster, especially on non- webkit browsers [(x)](#links)

1. In complex loops it's natural to leave the data calculation logic for JavaScript and keep the templates clean.

Look for the logic on the [customer listing](https://github.com/moot/riotjs-admin/blob/master/src/ui/customers.js). The single template is rendered multiple times on a loop and the `width` property is calculated with JavaScript and the [template](https://github.com/moot/riotjs-admin/blob/master/index.html#L11) remains simple.


## Routing

Routing or view switching is a core feature in single-page application. It's one of the main things that defines a framework. The demo application performs routing on the presenter layer as follows:


~~~ javascript
// All links that start with "#/" calls $.route()
$(document).on("click", "a[href^='#/']", function() {

  // $.route changes URL, notifies listeners and deals with back button
  $.route($(this).attr("href"));

});


// Listen to URL changes
$.route(function(path) {

  // Call API method to load stuff from server
  app.load(path.slice(2));

  // Remove is-active CSS class, that deals with page switch animation
  $(".page.is-active").removeClass("is-active");

});

// assign is-active class name after server responds
app.on("load", function(view) {
  $("#" + view.type + "-page").addClass("is-active");

});
~~~

The above code assumes that the API supports a generic `load` method and has a "load" event to signal when the load finishes. The returned `view` has a type parameter that we use to grab the correct node from the page and assign a CSS class "is-active" for it. The page switching animation is implemented with CSS transition.

The actual handling of the view is dealt by a view specific different presenter. The above code only handles the switch.

The ability to load new data from the server is designed on your API that the presenters can take advantage of. Here the `$.route` behaviour is just a thin layer above the API to deal with the back button. It's completely on the presenter layer. The API can focus on the business logic only unaware of the web layer.




## jQuery

jQuery exists because the vanilla DOM is a complete disaster to work with. jQuery does a massive cleanup by exposing the DOM for the page developer in an elegant and friendly way.

There are other implementations [(x)](#links) too, but the real credits go to John Resig. He designed the API, which has become a standarad to set a target for other implementation.

Feel free to use any of the implementations but the advantages of using jQuery itself are following

- best cross browser support, including IE6
- biggest test suite, most reliable
- already present on a website, possibly cached from a CDN

jQuery is currently found on 56% of all websites and 92.0% of all the websites whose JavaScript library is known [(x)](#links).

jQuery API is a perfect match for Riot. It's an ultimate tool for building presenters. The jQuery selectors provide a light way to bind the model to the view. And there can be multiple presenters dealing with the same set of HTML elements withtout them being aware of each other. It doesn't break modularity.

Id's and class names provide a natural mechanism to hook functionality, just like you can hook styling with CSS.


## Two-way data binding

Two-way data binding is the opposite of what jQuery does. You explicitly define the behaviour for a HTML element. Think inline style or an `onclick` attribute. It breaks modularity since the logic is hardcoded to an element and the behaviour is not defined inside a module.

When the behaviour is attached to the element from outside using a jQuery selector the architecture becomes modular. You can add and remove features without any impact to the other parts of the application.

Think of a new employee for example. One can build a separate module, select the same elements from the view and bind new functionality. And this module can later be removed. This is simply not possible with two-way binding because of the hardcoded nature – other team members will be affected.

This may seem like a small syntaxical detail but it's really about the big picture. Can you attach new features without touching the other parts of the client?

Two-way data binding is best suited for simple CRUD apps and quick prototypes. Riot will never embrace two-way data-binding since it violates modularity.


## Module interface

Now let's focus on coding. We want a simple way to extend the application core with modules. Here's a nice `$.observable` trick to split the application into loosely-coupled modules:


``` js
// works on client and server
var top = is_node ? exports : window,
  instance;

top.admin = $.observable(function(arg) {
  if (!arg) return instance;

  if ($.isFunction(arg)) {
    admin.on("ready", arg);

  } else {
    instance = new Admin(arg);

    instance.on("ready", function() {
      admin.trigger("ready", instance);
    });

  }

});
```

The above code expopses a single global variable `admin` that can be used as follows:

``` javascript
admin(function(api) {
  // module #1 logic, API is given as argument
})

admin(function(api) {
  // module #2 logic
})
```

All UI functionaly on the demp application are wrapped inside modules like this. Now after you have all code wrapped inside the modules you need to launch the application:


``` javascript
admin({ page: location.hash.slice(2), root: $("body") })
```

This will start the application with the configuration given on the argument and all the modules are called. This is typically called after the DOM has been loaded on the bottom of the page or inside jQuery's `document.ready`. The given argument typically contains the initial path of the application as well as the root element.

You can access the API after the initialization as follows:

``` javascript
var api = admin(); // get access

api.load("customers"); // call an API method
```

You can try that on the JavaScript console. All features that you can do with the UI are also available on the API. This is ensured by the strict separation of API and presenter modules.

Now your application is nicely split into decoupled modules and there is a simple way to add new modules. Each of your team members has only one, global `admin` method to use and the full API is given as the first argument to the module. The modules are isolated and can be freely added / removed without breaking other parts of the application.

Finally, the module interface is "white labeled". You can name the crucial module interface after your application and there is no 3rd party framework to force the naming scheme. Much cooler!


## Application lifecycle

When the application starts a sequence of events happen. Here are typical steps:

1. The call to server is made to load the initial view data
2. The API is fully constructed
3. The modules are initialized
4. A "load" event is fired with the returned initial data

An application that requires authentication needs an extra step handle unsuccesfull logins.

Now let's look at the code:

``` javascript

// 1. load initial data from server
backend.call("init", conf.page).always(function(data) {

  // 2. construct API
  self.user = new User(self, data ? data.user : {}, backend);

  // 3. all ready --> modules are loaded
  self.trigger("ready");

// init success
}).done(function(data) {

  // 4. load event
  self.trigger("load", data.view)

// init fail
}).fail(function() {

  self.user.one("login", function(data) {
    $.extend(self.user, data.user);

    // 4b. load event after succesfull login
    self.trigger("load", data.view);

  });

})
```

The modules are initialized on the "ready" event after which they listen to all the events that occur after on runtime.




# Extensions (on later doc?)

- standalone tools, something to use and not extend
- usable everywhere, not just Riot (it's vanilla anyway)
- extend jQuery if it's DOM related
- if it's functionality, make it a function! there is a tendency to build unnecessary modularity around a simple function,
- Riot is just a list of functions, they just extend the jQuery space and not global space
- a list of functions and patterns that you can copy and paste, perhaps modify a bit
- I'd like to see something like Gist but with better discoverability (search, tags etc)
- Anyway, feel free to just copy code directly from the demo app (bootstrappting, promises)



# Testing (on later doc?)

The driving reason to use Passive View is to enhance testability


"Humble View"
Humble Object - any object that is difficult to test should have minimal behavior.
http://martinfowler.com/eaaDev/uiArchs.html#Model-view-presentermvp

- models
- presenters
- views: CSS in different states



- [MVP passive view](http://martinfowler.com/eaaDev/PassiveScreen.html)

