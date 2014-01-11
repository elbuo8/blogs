# Building a Mobile App on a weekend a publishing it! (hackathon hack?)

Ever been to a hackathon wanting to come out with something published on one of the mobile markets?
With this tutorial I hope to provide you some tools that can help you get up and running in just a few minutes.

I will assume you have some [basic Angular knowledge](http://docs.angularjs.org/tutorial/step_01) and I will jump to introduce some building blocks which will see more in depth in the second part of this blog.


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
to either set up a reverse proxy(too nerdy?) to that folder or use Python's SimpleHTTPServer module. In my personal setup I have the following alias:

```bash
$ alias serve='python -m SimpleHTTPServer 3000'
```

Then just visit the address and voilà. Same application, running on your browser.

## Angular Basics

**$routeProvider** is Angular's router. It's a third party module that doesn't come in the code but Ionic already included it for us. 

```js
$routeProvider.when('/home', {
  templateUrl: 'templates/app.html',
  controller: 'AppCtrl'
});
```

If you notice, angular will bind a template to a controller's scope when using the router. If you navigate to **index.html** you will notice line 41 contains the following:

```html
<ng-view></ng-view>
```

Angular also has the concept of **Services**. These can be custom and easily injected into different modules thanks to Angular's Dependency Injection. 
Here I will provide a preview of the Service we will be using on the next part of the tutorial.

```js
.service('Playlist', ['$http', '$q', function ($http, $q) {
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
}])
```

In the next part of this blog, we will build a music player that pulls songs from Hype Machine's Popular Playlist for Android and iOS.
