# Simple SMS Reminders

I'm trying to drink more water starting today. There are several reasons behind it, but the top one is to make my mom happy. Mothers are somewhat hard to please. At least mine.

I have attempted this in the past but didn't end up well. Back in my first attempt, I had alarms set up at every single hour. This obviously didn't work since alarms are a bit annoying. I would be in a meeting or having a conversation and my water alarm would start ringing, awkward...

![](http://i61.tinypic.com/qys20h.png)

Since I still want to accomplish this, and alarms should not step in the way of a hacker, I built a more simpler system. I wrote a very little script which will send me an SMS using SendGrid while my own computer is on at some specific hours. Now, why would it work only when your computer is on you my ask? My logic is, if my computer is on, chances are that I will be sitting in front of it and water is completely obtainable.

Lets get to the implementation of it! I first wrote my basic Python template in a file called water_reminder.py:

```python
import sendgrid

sg = sendgrid.SendGridClient('api_user', 'api_key')

mail = sendgrid.Mail(subject='Drink YO WATAAAAA!', text=' ', from_email='yamil@sendgrid.com', to='my-phone-numba@messaging.sprintpcs.com')

print sg.send(mail)
```

Notice that you can get this from the docs of the [Python-Library](https://github.com/sendgrid/sendgrid-python). The one thing I want to bring up is the fact that my **to** address is my actual phone number's SMS address. Yes... I use Sprint... Don't judge me. If you run this basic script, you shall receive an SMS! SMSs over email :O For health!

![](http://s3.amazonaws.com/giphymedia/media/VsuowsrlIuhIA/giphy.gif)

Now lets get a bit more complicated. Append the following line to the top of your file:

```python
#!/usr/bin/env python
```

This line will let us know which Python interpreter we should use to evaluate the script. The goal is to make a script executable. To finish this process, you will need to run the following command on the file:

```bash
$ chmod +x water_reminder.py
```

Now you can run the script by running the following command:

```bash
$ ./water_reminder.py
```

BOOM! The rest is easy as PI. We can throw this script in a cron and it will execute whenever we want it. If you don't know how to set up a cronjob, fear not! Lets start by typing:

```bash
$ crontab -e
```

This will bring up a file were you can specify the jobs that you want to run in a time basis. In my case my file looks like this:

```bash
0 10 * * * /Users/yamilasusta/scripts/water_reminder.py >> /tmp/cronlog.out
0 11 * * * /Users/yamilasusta/scripts/water_reminder.py >> /tmp/cronlog.out
0 12 * * * /Users/yamilasusta/scripts/water_reminder.py >> /tmp/cronlog.out
0 13 * * * /Users/yamilasusta/scripts/water_reminder.py >> /tmp/cronlog.out
0 14 * * * /Users/yamilasusta/scripts/water_reminder.py >> /tmp/cronlog.out
```

This basically says that I want to receive an SMS everyday at 10AM, 11AM, etc. I can check why message wasn't delivered by checking the log in /tmp/cronlog.out.

![](http://i61.tinypic.com/35bxgd5.png)

And this is the final output! Now you can drink a lot of water like me. If you loved or hated this post, please let me know! If you have better ways, I'm always ears :)
