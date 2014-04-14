# How to Send Email with Go

A while ago, I found myself interested in learning [Go](http://golang.org). My regular approach to learn something, is building something. For Go, I decided I would learn by implementing the [SendGrid-Go](https://github.com/sendgrid/sendgrid-go) library. Initially I searched for a SMTP client that would allow everything [SendGrid's SMTP API](sendgrid.com/docs/API_Reference/SMTP_API/index.html) supports, but I was not very lucky. Therefore I decided to implement my own ([smtpmail](https://github.com/elbuo8/smtpmail)). After that I decided to build the SendGrid-Go library on top of my first library and extend it to it would support the [Web API](http://sendgrid.com/docs/API_Reference/Web_API/mail.html) too.

![gopher](http://dave.cheney.net/wp-content/uploads/2013/01/golang-gopher-figurine.jpeg)

So, on with the example. It's simple to use. First you will need to download and install the library (and its dependencies) by running the following command:

	go get github.com/sendgrid/sendgrid-go
	
From the fragment below, you can observe that in order to initialize the client, you must pass your [SendGrid](http://sendgrid.com) credentials. Afterwards, just create a message object and set some properties like destination and sender. Then use the client to send the message. Magic. If everything went according to plan, you won't receive an error!

	import (
    	"fmt"
    	"net/mail"
    	"github.com/sendgrid/sendgrid-go"
	)

	func main() {
    	sg := sendgrid.NewSendGridClient("sendgrid_user", "sendgrid_key")
    	message := sendgrid.NewMail()
    	message.AddTo("yamil@sendgrid.com")
    	message.AddSubject("My first email!")
    	message.AddText("Sending Email from Go using SendGrid")
    	message.AddFrom("yamil.asusta@sendgrid.com")
    	if r := sg.Send(message); r == nil {
        	fmt.Println("Email sent!")
    	} else {
        	fmt.Println(r)
    	}
	}
	
There are three ways to send emails using this library. The SendAPI will use the Web API meanwhile SendSMTP will use SMTP as its transport. Additionally, the Send method will try to use SendAPI, if it fails, it will fallback to SendSMTP. 

There is a more in-depth documentation found the [GitHub repository](https://github.com/sendgrid/sendgrid-go). 

Feel free to give [me a shout](https://twitter.com/elbuo8) if something blew up or just to say hi! I'm also going to [Gophercon 2014](gophercon.com) so if you are going, I would love to meet up.