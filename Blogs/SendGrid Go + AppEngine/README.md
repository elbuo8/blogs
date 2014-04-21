# SendGrid-Go + AppEngine

Are you a AppEngine user? Do you like Go? Did you know there
is a super service to send email with? [That is also using
Go for some of its stuff](https://sendgrid.com/blog/convince-company-go-golang/)? If you didn't, you are in luck!
SendGrid is here for all of your needs. As it turns out,
SendGrid is a partner with Google and its super easy to get
up and running with SendGrid on AppEngine using Go!

## Getting Started

First, register an app in [AppEngine](https://appengine.google.com/start/createapp) for demo purposes!

If you don't have a SendGrid account, today is your lucky day! Head on over to our [Google
Partnership page](http://sendgrid.com/partner/google)
to find out more of the benefits.

Then modify your
[PATH to include the Go AppEngine binaries](https://developers.google.com/appengine/docs/go/gettingstarted/devenvironment).
After doing this, you shall have the **goapp** binary ready to run. To be sure you can run the following command:

```bash
which goapp
```

It should print out the location of the **goapp** binary.

## Installing SendGrid

```Go
go get github.com/sendgrid/sendgrid-go
```

DONE! This will be available for all your future apps
if you [set up your GOPATH](http://golang.org/doc/code.html#GOPATH)
correctly.

Installing packages in Go is somewhat magical... You just install and there is no feedback.
If you are a bit more anal like me, and you like to have [Version Numbering](http://en.wikipedia.org/wiki/Dependency_hell#Solutions),
you should check out [Godep](https://github.com/tools/godep).

## Simple App

For this example I will use [Martini](https://github.com/go-martini/martini), my favorite Go Web Framework.
If you want to see how would this look like in any other framework (or
simply vanilla Go) I'd be more than glad to show you. Just [ping me on twitter](https://twitter.com/elbuo8).

So go ahead and run:
```Go
go get github.com/go-martini/martini
```

Now lets add some boilerplate code.

```Go
package demo

import (
	"appengine"
	"appengine/urlfetch"
	"github.com/go-martini/martini"
	"github.com/sendgrid/sendgrid-go"
	"net/http"
)

func init() {
	sg := sendgrid.NewSendGridClient("SENDGRID_USERNAME", "SENDGRID_PASSWORD")

	m := martini.Classic()

	m.Get("/:email", func(r *http.Request, params martini.Params) string {
		context := appengine.NewContext(r)
		sg.Client = urlfetch.Client(context)
		email := sendgrid.NewMail()
		email.AddTo(params["email"])
		email.AddSubject("Hello!")
		email.AddText("This was sent from your sample app!")
		email.AddFrom("yamil@sendgrid.com")
		email.AddFromName("Yamil @elbuo8")
		if e := sg.Send(email); e == nil {
			return "Sent!"
		} else {
			context.Infof("%v", e)
			return "Oups"
		}
	})
	http.Handle("/", m)
}
```

First of all, notice that the package is *demo* instead of *main*. That is because **AppEngine** already
provides us with a *main* package. Then we initialize our SendGridClient by passing our credentials in.
Once that is done, using Martini's simple routing options we create a simple route which will take in 1
parameter in the URL. This parameter should be an email since we are going to use it to be the recipient
of the demo.

Something also worth noting is the following:
```Go
context := appengine.NewContext(r)
sg.Client = urlfetch.Client(context)
```

AppEngine has the concept of [Contexts](https://developers.google.com/appengine/docs/go/reference). In order to perform an HTTP request on AppEngine's infrastructure,
a context needs to be created. Once you have such Context, you can perform logging, HTTP requests and much more. Since the SendGridClient needs to perform an HTTP
request, the HTTP client that it uses needs to be updated to the one using the Context. Thats where the [urlfetch](https://developers.google.com/appengine/docs/go/urlfetch/) package comes in handy.


## Deploying and Testing

Phew, we are almost at the finish line. We are going to need a **app.yaml** file for deployment.

```yaml
application: your-app-name
version: 1
runtime: go
api_version: go1

handlers:
  - url: /.*
    script: _go_app
```

Then, simply run:
```bash
goapp deploy
```

And your simple application will be compiled and uploaded! Head on over to your app and "test" it out!
