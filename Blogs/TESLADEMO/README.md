# Transactional Templates and Demos

If you haven't been living under a rock, you probably know that SendGrid released [Transactional Templates](https://sendgrid.com/templates) recently. I've been a been using it for a while specifically for my API demos. It makes it super simple for me customize emails for a specific school or crowd on the fly without touching any code. With Transactional Templates I don't ever touch my code.

Here is how my code looks like:

```py
import sendgrid
import os

sg = sendgrid.SendGridClient(os.getenv('SG_USER'), os.getenv('SG_PWD'))
mail = sendgrid.Mail(subject='10', reply_to='hi@sgdemos.com', html='Happy Hacking!', text='Happy Hacking', from_email='yamil@sendgrid.com', from_name='Yamil @elbuo8')
mail.add_to('someone@life.com')
mail.add_filter('templates', 'enable', 1)
mail.add_filter('templates', 'template_id', 'fd6ecf80-fc43-46da-80b0-f3e26777b514')
sg.send(mail)

```

Now if I want to go to [HackRU](http://hackru.org) and give a demo, I don't have to change anything in my code. I can go to the Templates interface and activate the template that I setup for HackRU.

![lol](http://i60.tinypic.com/200bt6g.png)


When I send my email, it will look like this:

![lol](http://i62.tinypic.com/dlt21c.png)

Now, next weekend I'll probably head on over to hackNY, I just need to activate that template and I'm done.

If you notice, the only thing that matters is the ID of the template :)

Convenient right? Anyone can start using the Template Engine! If you have any issues or questions, feel free to poke me [@elbuo8](https://twitter.com/elbuo8).
