# Gist me, please?

Do you find yourself writing technical blog posts in Markdown and exporting them to HTML? Do you end up writing [GitHub Flavored Markdown](https://help.github.com/articles/github-flavored-markdown) and then exporting to <code> blocks or something similar? I know I used to do that quite often. Thats the reason I created [Propagandist](https://github.com/elbuo8/propagandist).

Propagandist is a simple tool which will parse out your Markdown and convert it into HTML. But the cool part is that every Code Snippet that it finds gets replaced with a Gist for it. This produces a central Gist with all the snippets which users can jump into. Propagandist is built on top [Gost](https://github.com/elbuo8/gost) which is a simple wrapper for the [GitHub Gist API](https://developer.github.com/v3/gists/). They were both written in Golang on one of my train rides (I feel very productive in planes and train rides for some reason).

Here is a simple guide on how to get up and running with Propagandist.

### Installation:

```bash
go get github.com/elbuo8/propagandist
```

Done!

### Usage:

* -f="original.md"         // REQUIRED
* -o="output.html"         // Output filename. If not provided, STDOUT will be used
* -p=false                 // Determine if the Gist will be Public or Private, Private by Default
* -n="Name of the Gist"    // Name of the GistFile generated
* -d="Description of Gist" // Simple Description

For example:

```bash
$ propagandist -f="original.md" -o="publish.html"
```

And that will provide me with a ready to go WordPress markup. We use [Embed GitHub Gist](http://wordpress.org/plugins/embed-github-gist/) here at SendGrid therefore Propagandist uses that format for the Gists. But it can be easily modified to work with different styles.

If you think this is a cool project and want more features added to it, you know where to find me! It was originally developed to make blogging easier for me. If it can help anyone out there, I'd be super happy :)

Disclaimer: This post was generated using Propagandist! (Inception ah)
