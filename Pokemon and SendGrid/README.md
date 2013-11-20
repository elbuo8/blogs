#Pokemon and SendGrid

Who didn't play a Pokemon game when they were young? I know I did. Blue, Yellow, Silver and Gold. The good old days. Recently I've seen a lot of hype around the new games X and Y. It is for this reason that I decided to build my SendGrid(link) API demo around it.

The dynamic is pretty simple. For every email the audience sends to the application (Maybe it should say to me?), a little mail will fly towards the Pokemon performing damage to it. At some point the Pokemon dies and I will have a winner.

[image/gif]

Also, for every email the application receives, it replies to the sender with the amount of damage they performed and a small message regarding the status of the Pokemon.

With this simple demo I cover both the Inbound Parse Webhook(link) and Outbound Mail API(link) which are dead easy to use with all of the wrappers we have.

Now, on with the simple technical details. I built this using HTML5 canvas which I wanted to try out for the longest time. It runs on top of node.js. 

Canvas is pretty powerful and simple to get started on. I will write a blog post dedicated to HTML5 canvas once I clean up my demo's code.