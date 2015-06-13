---
layout: post
title:  "Developing Commands"
date:   2015-06-13 10:16
categories: tmux entr vim efficient-workflows
---

I had a problem recently where I needed to explore an [Elastic][Elastic] database
to figure out why some code wasn't working quite right. Since Elastic is fully
REST based, [curl][curl] is the first tool that came to mind, and a quickly
`curl http://<site>:<port>/<cluster>/<item>/_search` later, and I got a massive
dump of JSON data!

Clearly this was going to take doing to get right. Me being the right kind of
[lazy programmer][lazy], I felt the need to come up with a system of quickly
iterating between modifying the command and running it - pressing up, backsapce,
and enter every time just wasn't going to good enough. In my mind there were
three problems:

1. Editing the command
2. Executing the command
3. Seeing the results

## Editing the command

This was the easiest problem to solve. As an avid [vim][vim] user, there was no
other option. So I quickly setup the following file:

{% highlight bash %}
#!/bin/fish
curl http://site:1234/cluster/item/_search -d '
  {
    "query": {
      "bool": {
        "should": [
          "term": {
            "arraySearch": "itemValue"
          }
        ]
      }
    }
  }
'
{% endhighlight %}

## Executing the command

I found a nifty little program called [entr][entr]. With `entr`, I'm able to
watch my shell script and re-run it every time a change is saved via:

{% highlight bash %}
echo script.sh | entr -r fish -c '. script.sh'
{% endhighlight %}

## Seeing the results

This is great, but I wanted to view everything on one screen. Multiple terminal
tabs or windows wasn't enough, so I turned to [tmux][tmux]. `tumx` allows me to
split a single terminal (in full screen mode, just for fun) into multiple. I
split vertically, then horizontally so that I could have vim editing the file in
the upper left, a scratch terminal lower left, and `entr` running on the right
with full results. Every time I saved vim, I instantly see the result so that I
can figure out what to do next.

[Elastic]: https://www.elastic.co/
[lazy]: http://blog.codinghorror.com/how-to-be-lazy-dumb-and-successful/
[curl]: /tools/curl
[vim]: /tools/vim
[entr]: /tools/entr
[tmux]: /tools/tmux
