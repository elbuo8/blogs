# Python + Flask

*"Flask is a microframework for Python based on Werkzeug, Jinja 2 and good
intentions."*

And by micro, they mean it's super easy to use. Batteries are not included.
But you can inject batteries as you go.

Lets get started. Assuming you have your environment set up properly, you
should have either *pip* or *easy_install*. For the sake of versioning we
will use **virtualenv**.

```bash
$ pip install virtualenv
// or
$ easy_install virtualenv
```

You may or may not need to use **sudo** when running that. Lets follow up
by creating a virtualenv for a project called TweetBot. Most of my tutorials
are based on Twitter since most people are familiar to it.

```bash
$ virtualenv tweetbot
$ cd tweetbot
$ source bin/activate
$ pip install flask sendgrid
```

Small recap of the commands. Source will actually activate the virtualenv
in order for us to have a contained python version. Everything we do now,
will be contained to this virtual python environment. Whenever you want to
exit the virtualenv, just write **deactivate**.

Like previously mentioned, we are going to be building a Twitter clone.
But its just going to be a simple
[RESTful API](http://en.wikipedia.org/wiki/Representational_state_transfer).

The the starting boiler plate will look like the following:

```python
from flask import Flask, request, jsonify
from random import randint
from sendgrid import Mail, SendGridClient

app = Flask(__name__)
app.debug = True

tweets = []

#put code here

if __name__ == '__main__':
  app.run()

```

If you notice, Flask contains everything we need out of the box.

The POST endpoint will be used to save our tweets. Quick
note, our database for this example is an array so, nothing fancy haha.
On the other hand, the GET where no parameters are included, will return
a list of all tweets.


```python
@app.route('/tweet', methods=['POST', 'GET'])
def handle_create():
  if request.method == 'POST':
    new_tweet = {"id": randint(), "text": request.form['tweet']}
    tweets.append(newtweet)
    return jsonify(newtweet)
  else:
    return jsonify({"tweets": tweets})
```

Now gets handle tweets individually. Either return a specific tweet (GET),
modify a tweet (PUT), or delete a tweet (DELETE).

```python
@app.route('/tweet/<int:id>', methods=['PUT', 'GET', 'DELETE'])
def handle_single_tweet(id):
  for tweet in tweets:
    if tweet['id'] == id:
      if request.method == 'GET':
        return jsonify(tweet)
      elif request.method == 'PUT':
        tweet['text'] = request.form('text')
        return jsonify(tweet)
      else:
        removed = tweet
        tweets.remove(tweet)
        return jsonify(removed)
  return jsonify({"error": "Not found"})

```

*Notice, this can be organized in a cleaner way, but for the sake of showing
everything step by step, I added some extra verbose.*


Now we have a way to create, view, edit, delete tweets! Start playing around!
This was initially built for a workshop I've been giving as practice for
Puerto Rico's upcoming hackathon, [hackPR](http://hackpr.io/).

## Bonus! Adding Tweets via email!

First, lets add the following route.

```python
@app.route('/tweet/emailhook', methods=['POST'])
def email_tweet():
  new_tweet = {'id': randint(), 'text': request.form['subject']}
  tweets.append(new_tweet)
  return jsonify(new_tweet)
```

This will basically receive a POST request from SendGrid's [Inbound
Parse API](http://sendgrid.com/docs/API_Reference/Webhooks/parse.html).

So go ahead and send yourself an email and the subject will be added to
your twitter!

For the full source code, refer
to my [GitHub](https://github.com/elbuo8/flask-tutorial).

If you want me to drop by and give this workshop for your peeps, I'd
be more than glad to do so! Send me a tweet [@elbuo8](https://twitter.com/elbuo8) and we can sort it out :)
