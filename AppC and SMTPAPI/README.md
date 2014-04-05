# Sending Email from Appcelerator Titanium

Did you know you can build native app using web technologies?
[Titanium Appcelerator](http://www.appcelerator.com/titanium/)
does exactly that. And its super straight forward to get up
and running.

Recently, I found myself creating a library that would make it
super easy for Titanium developers to send email using none other
than our favorite SMTP provider, SendGrid!

Lets get to it!

### Installation

Since Titanium uses CommonJS compliant modules, all you have to do
is drop **[tisendgrid.js](https://raw.githubusercontent.com/sendgrid/ti.sendgrid/master/app/lib/tisendgrid.js)**
in your **lib** folder. That's it!

### Example

```javascript
var sendgrid = require('tisendgrid')('SENDGRID-USERNAME', 'SENDGRID-PASSOWORD');
sendgrid.send({
    to: 'john@email.com',
    from: 'doe@email.com',
    subject: 'Hello!',
    text: 'Hello again!'
}, function (e) {
    if (e) {
        console.log(e); // Email wasn't sent
    }
});
```

Due to the CommonJS compliance, you are able to **require** a module like Node.js and such.
It's as simple as that! The library has several examples and documentation in its
[GitHub repository](https://github.com/sendgrid/ti.sendgrid).

### SMTPAPI-Example

Lets go into a more complicated example. Lets use some SendGrid Apps (or Filters).
For this demo lets use the [Template Filter](http://sendgrid.com/docs/API_Reference/SMTP_API/apps.html#template).
For this we need to use the [SMTPAPI Header](http://sendgrid.com/docs/API_Reference/SMTP_API/index.html).
Thankfully, this is already built in to the library. Lets get to the code!

```javascript
var sendgrid = require('tisendgrid')('SENDGRID-USERNAME', 'SENDGRID-PASSOWORD');
var email = sendgrid.Email({
  to: 'john@email.com',
  from: 'doe@email.com',
  subject: 'Welcome!',
  text: 'This is inside of the template!'
});

email.addFilter('template', 'enable', 1);
email.addFilter('template', 'text/html',
'<html><head></head><body bgcolor=\'blue\'><div style=\'width:200px\'
bgcolor=\'#FFF\'><% body %></div></body></html>');
sendgrid.send(email, function (e) {
    if (e) {
      console.log('Oups :( : ' + e);
    }
});
```

Simple enough ah? Just got got yourself a simple template.

If you have any questions or concerns, feel free to reach out to me at [@elbuo8](https://twitter.com/elbuo8).

Also, special thanks to [@ricardoalcocer](https://twitter.com/ricardoalcocer) for the initial work on the library.
