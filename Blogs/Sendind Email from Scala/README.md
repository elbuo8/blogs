# Scala + SendGrid

Are you Java developer looking to use the JVM in different ways? Maybe looking at Clojure? If you are
looking into Scala, then this post might be able to help you out.

Scala runs in the JVM. If you weren't aware, all of your Java code can run in Scala. Pretty awesome right?
For example, recently, the SendGrid JAR went through some major changes in order to make it more robust. You
can use the old JAR as well as the new one because at the end, it all becomes bytecode for the JVM.

Lets learn a bit of the difference in syntaxes when coming from Java and using the SendGrid JAR in order to, you know, send email.

Importing the library in Java is as simple as:

```java
import com.sendgrid.*;
```

Its Scala counter part is:

```scala
import com.sendgrid._;
```

Basically *_* implies wildcard, while in Java its _*_. Also, in Scala, there is no need for semicolumns. They are completely optional.

Instatiating the SendGrid objects is quite similar. The one thing that changes is the fact that Scala has *Type inference* and there is no need to specify the type of the object.

```java
SendGrid sg = new SendGrid("api_user", "api_key");
SendGrid.Email mail = new SendGrid.Email();
```

VS

```scala
var sg = new SendGrid("api_user", "api_key");
var mail = new SendGrid.Email();
```

The rest is the same. Do keep in mind that in Scala the keyword *var* states that such object is **Mutable**. If you want a **Inmutable** object, instatiate it using the keyword *val*.

Another cool thing about Scala is that you can invoke methods using space instead of the usual **dot notation**. For example:

```scala
sg.send(mail)
// =
sg send(mail)
```

They are completely equal.

This is definately not a comprehensive begginer guide. But I would be super stoked to find out that
it got you a bit interested in Scala. If so, give me a shout out @elbuo8 :)
