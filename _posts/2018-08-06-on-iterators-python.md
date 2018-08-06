---
layout: post
title: On iterators and generators, or how to feed your pythons in Python
tags: python
---
&nbsp;

<em>**Disclaimer**: All characters appearing in this work are fictitious. Any resemblance to real pythons, living or dead, is purely coincidental. This is a very informal treatment of iterator and generator concepts in the Python language. I will be glad to hear any suggestions on how to make the metaphors used here more vivid and precise. Meanwhile, If you need to dive deeper I recommend checking this [Datacamp article](https://www.datacamp.com/community/tutorials/python-iterator-tutorial). </em>

<del>Imagine you are a sales manager at Parch and Posey</del>.

Imagine you are a freelance herpetologist (a person who studies reptiles and amphibians) and you have five pythons in one big aquarium at your home. Every morning you need to manually feed them: pull every python out of the aquarium, give food to it, and put it back.

<p align="center">
<img src="/images/both_1.png" height="270" width="350">
</p>

How messy this tedious process can become sometimes! In Python it would look like this:

{% highlight py %}
aquarium[0].feed()
aquarium[1].feed()
aquarium[2].feed()
aquarium[3].feed()
aquarium[4].feed()
{% endhighlight %}

Now, imagine what would happen if you had 100 pythons! It will be hard to look after every animal. If you by mistake confuse the indices, some sneaky creatures can get double servings, while others will get none:

{% highlight py %}
...
aquarium[55].feed()
aquarium[55].feed() # this serving should go to the 56th python
aquarium[57].feed()
...
{% endhighlight %}


Luckily for you, there is a mechanism built in your aquarium, which can give you one python at a time, so you don’t have to manually sort them out. This mechanism is called the <ins>**for-loop**</ins>. There is a catch, though. Have you ever seen how five snakes in one aquarium can look like?

That twisted and convoluted bunch is not so easy to handle, and that’s why you should explicitly tell your mechanism how exactly it should disentangle these lovely creatures. This is what the method <ins>**next**</ins> does. It specifies how to pick the next python you are going to feed. The <ins>**iter**</ins> method just returns the aquarium and, in our case, initializes some starting variables. You need to take apart the aquarium and insert these two methods. Your shiny upgraded aquarium will have a new look:


{% highlight py %}
class Aquarium:

    def __init__(self, pythons):
	self.pythons = pythons #all pythons
        self.num_of_pythons = len(pythons) # num of pythons

    def __iter__(self):
        self.current_python = 0
        return self

    def __next__(self):
        if self.current_python < self.num_of_pythons:
            python_to_feed = self.pythons[self.current_python]
            self.current_python += 1
            return python_to_feed
        else:
            raise NoMorePythons
{% endhighlight %}


Congratulations, you have just made your own iterable and iterator! In this case the aquarium is both iterable and iterator because it can iterate over itself. Now you can iterate over all your pythons with just simple two lines of code, saving yourself a lot of time and space, and be sure that no python will remain hungry:

{% highlight py %}
for python in aquarium:
	python.feed()
{% endhighlight %}

However, the previous solution looks rather complicated. You have to go to the aquarium internals and tweak them to serve your purpose. Of course, for such a ninja like you it is not a big deal, but what happens if you go for a trip leaving your precious animals to your husband? He is an English major and he does not want to get all technical. If the mechanism breaks, he won’t be able to fix it, so your pythons are in danger!

<p align="center">
<img src="/images/both_2.png" height="270" width="350">
</p>

Thank goodness, there is a special type of the iterator mechanism that is called <em>generator</em>, which can be used quite easily. You don’t need to internally modify your aquarium, you can just add some simple extension to it. This extension will help you to feed one python at a time and will take all the burden of remembering which pythons are still hungry and which are not. This aquarium generator extension can look like this:

{% highlight py %}
def python_generator(aquarium):
    num_pythons = len(aquarium)
    for i in range(num_pythons):
        python = aquarium[i]
	yield python
{% endhighlight %}


The <ins>**yield**</ins> keyword is an essential part of any generator. It 'freezes' the generator's internal state, so everytime you call the generator, it gives you the next python to feed. The generator can be used in the <ins>**for-loop**</ins> as well:

{% highlight py %}
for python in python_generator(aquarium):
	python.feed()
{% endhighlight %}


<p align="center">
<img src="/images/both_3.png" height="270" width="350">
</p>
