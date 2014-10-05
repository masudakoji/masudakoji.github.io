---
layout: post
title: "code sample"
description: ""
category: 
tags: []
---
{% include JB/setup %}


### Jekyllでシンタックスハイライトを使う
{% highlight ruby linenos %}
class Person
  def initialize(name)
    @name = name
  end
  
  def hello
    "Hello, friend!\nMy name is #{@name}!"
  end
end

charlie = Person.new('Charlie')
puts charlie.hello

# >> Hello, friend!
# >> My name is Charlie!
{% endhighlight %}
