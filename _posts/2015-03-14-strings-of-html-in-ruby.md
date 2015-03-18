---
layout: post
title: Strings of HTML in Ruby
category: programming
tags: [redactorjs, ruby-on-rails, story-vine]
---

In my previous blog post, I talked about how we wanted to use RedactorJS to give the users the same feel as writing a blog post. There were some questions that originated from this such as **1)** how would we sanitize the strings of html before saving it to the database, **2)** how to get rid of the tags when displaying a snippet/story title on a show-all page, and **3)** how to render the snippet or story with the HTML tags.

<!-- more -->

To solve the first problem, I found a gem called '[sanitize](https://github.com/rgrove/sanitize)'. The documentation allowed me to choose from varying levels of strictness for sanitazation. I went with the BASIC setting which "allows a variety of markup including formatting elements, links, and lists". I used it in my controller like so:
{% highlight ruby %}
  snippet_content = Sanitize.fragment(params["snippet"]["content"], Sanitize::Config::BASIC)
  @snippet.update(content: snippet_content)
{% endhighlight %}

The second problem was that on the show-all pages, snippets and story titles were appearing with their html tag.
{% highlight html %}
<p>This is a snippet.</p>
{% endhighlight %}

To solve this problem, I used the built-in ruby `strip_tags` method. ([source](http://api.rubyonrails.org/classes/ActionView/Helpers/SanitizeHelper.html))
{% highlight ruby %}
strip_tags("Strip <i>these</i> tags!")
# => Strip these tags!

strip_tags("<b>Bold</b> no more!  <a href='more.html'>See more here</a>...")
# => Bold no more!  See more here...

strip_tags("<div id='top-bar'>Welcome to my website!</div>")
# => Welcome to my website!
{% endhighlight %}

And to solve the final problem, I used the built-in `html_safe` method. It can also be replaced with `raw`, and it might even be better to use `raw` since `html_safe` will raise an error if the string is nil, while `raw` will not. ([source](http://api.rubyonrails.org/classes/ActionView/Helpers/OutputSafetyHelper.html#method-i-raw))

Of course, it should be noted that it is extremely important to sanitize all of the HTML strings before rendering them, especially if the HTML strings are from user input. We wouldn't want any [Bobby Tables](http://xkcd.com/327/).
