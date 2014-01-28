# QuickStart Part 2

On the [previous part of this blog](http://sendgrid.com/blog/mobile-app-hackathon-quickstart-guide/), I explained how to set up a basic [Ionic](http://ionicframework.com) project. By Ionic I mean an [AngularJS](http://angularjs.org) app which is ready to be deployed on a mobile device by using [PhoneGap](http://phonegap.com). 
On this part of the tutorial I will walk you through building a more complex application in no time. But before we begin, I must be clear that the resulting application was just built for tutorial/exploration purposes. By no means this application should be published on any market since I have released its source code [without license](http://www.codinghorror.com/blog/2007/04/pick-a-license-any-license.html). I had asked [Hype Machine](http://hypem.com) for permission on this and they were awesome enough to let me publish this.

##Markup

We need a header and a placeholder for the content. A template if you will. Using Ionic's [built in classes and elements](http://ionicframework.com/docs/components/) we can have a nice UI with just a few lines.

[gist]

Notice that we include **ng-view**. This is were we will render the actual content. Hence, it works like a placeholder.

For the actual content, lets use an [Angular Template](http://docs.angularjs.org/guide/templates). Markup is pretty simple. 

[gist]

Notice the {{}}? The values surrounded by {{}} are variables that binded from the controller. When we reach the controller code, you will notice that all variables in the template are also declared in the Player Controller. Whatever you change in the controller, it will be reflected in the view. Cool stuff huh?

##JavaScript

Lets move towards JavaScript now. From the last blog we had a simple router and the Playlist service. Lets create a new service which will be in charge of fetching the song streams. 

[gist]

I'm a huge fan of the [async library by caolan](https://github.com/caolan/async) therefore I figured that I could also inject it as a service by doing the following.

[gist]

Mind you, all you have to do is require it like you would require any JS file in your **index.html**.

And with that we are ready to build our controller! The controller will act as music player. It will interface with our different services to obtain songs and artwork to display on our view. This is where the template will become super useful since it will take care of pretty much everything regarding DOM manipulation.

[gist]

That is it! Have a look at the final product here. Feel free to clone it and run it in the emulator.