# Gophers can haz A/B Testing in Transactional Email

How much do you know about the awesome folks [@SendWithUs](https://twitter.com/send_with_us)? Here @SendGrid we are quite fans and quite frankly friends with them. They provide an amazing service, A/B over transactional email templates. If you haven't heard from them and you are a SendGrid customer, today is your lucky day since we have a [partnership](http://sendgrid.com/blog/sendgrid-announces-partnership-with-sendwithus/) that might interest you!

But enough about that, lets get a bit technical!

I noticed that SWU didn't have a Golang library. So I figured I could build them one and allow every Go developer out there to interact with their service without having to implement their own :)

Go get it! Get it?

```bash
go get github.com/elbuo8/sendwithus_go
```

How do I send an email?

```go
package main

import (
  "github.com/elbuo8/sendwithus_go"
  "fmt"
)

func main() {
    api := New("SWU_KEY") // One of your SWU Keys
    email := &SWUEmail{
        ID: "EMAIL_TEMPLATE_ID",
        Recipient: &SWURecipient{
            Address: "example@email.com",
        },
        EmailData: make(map[string]string),
    }
    err := api.Send(email)
    if err != nil {
      fmt.Println(err)
    }
}

```

<center>![magic](http://uproxx.files.wordpress.com/2013/05/gob-trick.gif)</center>

Can't get any easier than that!

The library does much more than sending emails. You can find the source [here](https://github.com/elbuo8/sendwithus_go) which contain tests that can work as examples! I will keep working on the library to support all possible endpoints.

If you have any questions, feedback or concerns, you know where to find me :)
