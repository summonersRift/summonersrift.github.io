---
published: true
---

<p class="intro"><span class="dropcap">T</span>his post talks about formatting and markup in jekyll. This is the intro paragraph of a post.</p>

<figure>
	<img src="{{ '/assets/img/touring.jpg' | prepend: site.baseurl }}" alt=""> 
	<figcaption>Fig1. - This is an example figcaption</figcaption>
</figure>

{% highlight html %}
<figure>
	<img src="{{ '/assets/img/touring.jpg' | prepend: site.baseurl }}" alt=""> 
	<figcaption>Fig1. - This is an example figcaption</figcaption>
</figure>
{% endhighlight %}

Kaperij lanterne rouge musette rund um koln bruges thor smash.  Nyvelocity pyrenees vande velde merckx. 


#### Code Snippet

{{ "{% highlight bash" }}%} <br><br>
code goes here<br><br>
{{ "{% endhighlight" }}%}



# Heading 1

## Heading 2

### Heading 3

#### Heading 4

##### Heading 5

###### Heading 6

<blockquote>This is an example blockquote in jekyll.</blockquote>

This is normal text without paragraph. 

{% highlight linenos%}

## Unordered List
* List Item
* Longer List Item
  * Nested List Item
  * Nested Item
* List Item

## Ordered List
1. List Item
2. Longer List Item
    1. Nested OL Item (4--spaces)
    2. Another Nested Item
3. List Item

# Heading 1

## Heading 2

### Heading 3

#### Heading 4

##### Heading 5

###### Heading 6
{% endhighlight %}


## Unordered List
* List Item
* Longer List Item
  * Nested List Item
  * Nested Item
* List Item

## Ordered List
1. List Item
2. Longer List Item
    1. Nested OL Item
    2. Another Nested Item
3. List Item

## Definition List
<dl>
  <dt>Coffee</dt>
  <dd>Black hot drink</dd>
  <dt>Milk</dt>
  <dd>White cold drink</dd>
</dl>



<p>
You'll find this post in your `_posts` directory - edit this post and re-build (or run with the `-w` switch) to see your changes! To add new posts, simply add a file in the `_posts` directory that follows the convention: YYYY-MM-DD-name-of-post.ext.
</p>

## Code Snippets:
Jekyll also offers powerful support for code snippets:
Add linenos at the beginning after the name of programming language if you want to include line number.
example: ruby, bash, cpp, etc.

{% highlight python%}
#{-%- highlight python -%-}
#clear all the dashes.
#{-%- endhighlight -%-}
{% endhighlight %}

{% highlight ruby linenos%}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Obaida')
#=> prints 'Hi, Obaida' to STDOUT.
{% endhighlight %}


Check out the [Jekyll docs][jekyll] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll's GitHub repo][jekyll-gh].

[jekyll-gh]: https://github.com/mojombo/jekyll
[jekyll]:    http://jekyllrb.com



Simple Image:<br>
<img src="{{ '/assets/img/touring.jpg' | prepend: site.baseurl }}" alt="">

<br>Code:
{% highlight html %}
<img src="{{ '/assets/img/touring.jpg' | prepend: site.baseurl }}" alt="">
{% endhighlight %}

