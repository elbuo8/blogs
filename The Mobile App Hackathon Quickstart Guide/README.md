# The Mobile App Hackathon Quickstart Guide

Ever been to a hackathon wanting to come out with something published on one of the mobile markets?
With this tutorial I hope to provide you some tools that can help you get up and running in just a few minutes.

This tutorial will assume you have some [basic Angular knowledge](http://docs.angularjs.org/tutorial/step_01). For those of you who don't know about it, Angular is a great front-end MVC Framework. It contains tools for animations, routing, templating between other kick-ass features. In this tutorial we will touch a little about routing and services.

## The tools
* [PhoneGap](http://phonegap.com/)
* [Angular](http://angularjs.org)
* [Ionic Framework](http://ionicframework.com/)

## Setup

Lets get started! First, make sure you have [npm](http://npmjs.org) installed. After that, run the following command:
```bash
$ sudo npm install -g ionic phonegap
```

#### Small sidenote, please have either iOS or Android emulators ready.

This will install both PhoneGap and Ionic CLI tools. Afterwards, we can run the following command:

```bash
$ ionic start tutorial
$ cd tutorial
$ phonegap run ios --emulator
```

You should see a sample application pop up in the emulator. If you browse the source in the www directory, you will find extensive documentation on how it works.
Notice that the Ionic generator generated everything you needed to deploy a mobile. For now, just worry about the **www** directory. My recomendation for fast development is
to either set up a reverse proxy to that folder or use Python's SimpleHTTPServer module. In my personal setup I have the following alias:

```bash
$ alias serve='python -m SimpleHTTPServer 3000'
```

Then just visit the address and voilà. Same application, running on your browser.

## Angular Basics

**$routeProvider** is Angular's router. It's a third party module that doesn't come in the Angular core but Ionic already included it for us.

```js
$routeProvider.when('/home', {
  templateUrl: 'templates/app.html',
  controller: 'AppCtrl'
});
```
Whenever a request is performed to **'/home'**, Angular will render the **app.html** template. It will also bind **AppCtrl** to the DOM of the template. This is pretty cool since it it gives us the ability to have multiple controllers per page. Each controlling a single part of the view. Talk about modular code.

Now, where would this template be rendered? Easy. Adding the following line in your **body** will tell Angular where you want to see that view.

```html
<ng-view></ng-view>
```

Angular also has the concept of **Services**. These can be custom and easily injected into different modules thanks to [Angular's Dependency Injection](http://docs.angularjs.org/guide/di).
Here's a preview of the Service we will be using on the next part of the tutorial.

```js
service('Playlist', ['$http', '$q', function ($http, $q) {
  return {get: function (params) {
    var deferred = $q.defer();
    $http.get('http://hypem.com/playlist/' + params.playlist + '/all/json/' + params.pagenum + '/data.json')
    .success(function (data) {
      deferred.resolve(data);
    })
    .error(function(data) {
      deferred.reject(data);
    });
    return deferred.promise;
  }};
}]);
```
Breaking it down, this Service is called Playlist. Using Angular's DI we imported [$http](http://docs.angularjs.org/api/ng.$http) and [$q](http://docs.angularjs.org/api/ng.$q) services. Service Inception. With them, we get a [HypeM's](http://hypem.com) playlist data and return it in a callback. The Playlist service is now ready to be injected into any other module.

At this point you should have at least the following files:

//gist of index.html
//gist of app.js

In the next part of this blog, we will build a music player that pulls songs from [Hype Machine's Popular Playlist](http://hypem.com/popular) for Android and iOS. The end result will hopefully look like this:

![hypem](/player)
