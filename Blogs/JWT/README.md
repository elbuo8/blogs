# JSON Web Tokens And Go

Recently, my buddies from [Blimp](http://getblimp.com) poked my interest about JSON Web Tokens. You can find very detailed 
specs about it [here](https://developer.atlassian.com/static/connect/docs/concepts/understanding-jwt.html), & [here](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).
In this post I want to guide you through implementing the equivalent of sessions but with JWT.

The traditional session approach usually requires the client to store some sort of value in its cookies while the server must have some sort of 
session storage where its stores that same value. For each request the client makes, the server has to make a network trip to check that the cookie's value
is in the session storage.

What if there was no need for a session store? What if you could generate a token that could contain all the necesary information to prove that the request
is coming from an authenticated user? You can, using JWT.

![MAGIC](http://img.pandawhale.com/94867-magic-gif-Shia-Imgur-MAH7.gif)

I will try to walk you through implementing this in your Go server. For this implementation I decided to go with [dgrijalva's](github.com/dgrijalva/jwt-go) implementation.

Before we begin, we need to generate RSA keys for our project which we will use to sign the tokens. To do so, run the following commands:

```bash
$ openssl genrsa -out demo.rsa 1024 # the 1024 is the size of the key we are generating
$ openssl rsa -in demo.rsa -pubout > demo.rsa.pub 
```

Now, for your application code, lets mimic a request which will receive a JWT.

```go
import (
  jwt "github.com/dgrijalva/jwt-go"
  "io/ioutil"
  "net/http"
  "fmt"
)

var (
  privateKey []byte
)

func init() {
  privateKey, _ = ioutil.ReadFile("Location of your demo.rsa")
}

func handleRequest(w http.ResponseWriter, r *http.Request) {
  token := jwt.New(jwt.GetSigningMethod("RS256")) // Create a Token that will be signed with RSA 256. 
  /*
  {
      "typ":"JWT",
      "alg":"RS256"
  }
  /*

  token.Claims["ID"] = "This is my super fake ID"
  token.Claims["exp"] = time.Now().Unix() + 36000

  // The claims object allows you to store information in the actual token.

  tokenString, _ := token.SignedString(privateKey)

  // tokenString Contains the actual token you should share with your client.

  w.WriteHeader(http.StatusOK)
  fmt.Fprintf(w, "{\"token\": %s}", tokenString)
}

```

Make sure you store the token in the client. I personally like to do it in Local Storage but its completely up to you.
From now on, all of your requests should contain the following header:

Authorization: Bearer <TOKEN HERE>

The header will be the only thing we need to identify if the user is someone we trust. Lets look at the code to parse the token out when a request is made.

```go
import (
   jwt "github.com/dgrijalva/jwt-go"
   "io/ioutil"
   "net/http"
   "fmt"
)

var (
  publicKey []byte
)

func init() {
  publicKey, _ = ioutil.ReadFile("Location of your demo.rsa.pub")
}

func authRequest(w http.ResponseWriter, r *http.Request) {
  token, err := jwt.ParseFromRequest(r, func(token *jwt.Token) ([]byte, error) {
    return publicKey, nil
  })
  if token.Valid {
    //YAY!
  } else {
    //Someone is being funny
  }
}

```

And there you have it. Of course the code here is far from optimal (no error checking etc) but it should hopefully give you
a nice picture of how to set up JWT in your Go application. Feel free to reach out if you want an extra hand or have a better way :)
