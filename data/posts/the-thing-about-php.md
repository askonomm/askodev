---
title: The Thing About PHP
date: 2023-10-18
slug: the-thing-about-php
status: published
---

Oh, PHP. The internet's most hated language. Yet, so many got their start with it, including me. 
While for the past 5 years I haven't been writing any PHP because I've been busy building software with 
[Clojure](https://clojure.org/), I've recently become a freelance consultant instead of a contractor that I've previously been and 
a lot of (the majority even) of work that comes my way is PHP. I don't mind, turns out it pays as much 
as Clojure does so long as you're not a 9-5 employee and can thus negotiate better rates.

But, do I like PHP? What is it exactly that I think about PHP? Would I use it for my personal projects?
I think about this often, especially now that I've learnt many other languages and have a better perspective.

## The Nostalgia Element

I can't deny that a lot of the positive outlook I have towards it stems most likely from nostalgia. While a 
lot of people like to point out that PHP was a horrible language 10+ years ago, which is when I started out with PHP, 
I don't share their views. Not because I don't agree with them, but because I was too stupid to know that it was bad at the time, 
and managed to create things with it just fine, despite its flaws, which came apparent to me much later.

I look back and remember being able to write code and deploy it _fast_ without a need for any build pipelines. It was
easy enough to be able to learn on your own without any help (at least that's how I did it) and there were jobs for it
_everywhere_. You could become a software engineer by just figuring out how WordPress themes worked. I did. It were 
simpler times.

## The Language

PHP the programming language is still, to me, a very pragmatic language. It's easy to write and 
easy to deploy and with the recent feature additions has become a pretty decent Java of the web. I mean that 
both in good and bad ways. 

_Good_ as in it has a sort-of decent type system now, which helps in writing more correct code and 
gives me IDE completion without needing to write doc blocks. I prefer this because doc blocks 
never get updated, and with a type system when the implementation changes, so do the IDE 
completions - no extra work needed.

_Bad_ in that it's increasingly becoming a standard to write over-engineered OO much like Java is, with its `FactoryBuilderContractInterfaceController` 
stuff, which while I have nothing really against OO is just stupid over-abstraction in my opinion. Navigating, for example, 
[Laravel](https://laravel.com/)'s code to find out how something under the hood is implemented is like trying to find a unicorn.

And bad also because the type system it has is very half-baked. You can't return an array of typed objects (e.g `array<Interface>`, you can only return 
`array`. There are no generics, and from what I understand for some odd reason [will never be](https://www.reddit.com/r/PHP/comments/j65968/comment/g7zg9mt/) even though [Hack](https://hacklang.org/) forked from PHP and 
managed to add them? 

Then, of course, the inconsistent core functions where the needle and haystack arguments are in an ever-changing order 
that you will never remember, but I give that one a pass since the IDE helps out with that.

## The Ecosystem and Tooling

While the language is not something I find terribly satisfying to use, mostly because it helps me write correct code 
only to a certain point and then the type system becomes more of a joke because of the aforementioned inability to return an array 
of typed objects and only a generic `array` (an array of what, exactly?), the ecosystem and tooling are actually really-really good.

There isn't an editor or IDE that doesn't support PHP. There are a ton of high-quality packages available on [Packagist](https://packagist.org/) 
and while some people seem to mention that PHP's de-facto package manager, [Composer](https://getcomposer.org/), is slow, I personally haven't 
seen any slowness at all nor have I had any issues with it. To me, it has simply worked, and worked well. I also really 
like how easy it is to publish and update PHP packages, by simply creating new tags for your library on Git and a new 
version of your library will become available instantly.

The community also seems to be quite vibrant, active and ready to help anyone who is struggling to figure something out. 
There is now a [PHP Foundation](https://thephp.foundation/) which hopefully steers the language in the right direction, and perhaps also tries to lessen the 
bad reputation the language has because while it's not the greatest language, I also do not think it deserves to be 
as hated as it is. 

## Would I use PHP for my own projects?

No. Well not as it is right now, at least. What matters more and more to me about programming languages are 
two things - the tooling and how well will it help me write correct software that doesn't come back to bite me in the ass later. 
PHP tooling is great, but the ability to write bug-free code isn't. Now, I'm no rockstar programmer, so maybe 
that's why, but I do know I am able to write a lot more bug-free code in a language like Rust, so I figure there must be something 
to it. Like perhaps the compiler being a lot more helpful and outright refusing to compile with bad code.

If PHP figured out how to do generics and add the ability to return an array of typed objects it would become a 
lot more attractive in my eyes, since then I should be able to safely write most, if not all, of my code, without having 
to do manual type checking within methods to verify that an array is indeed of expected type, which is both tiresome 
and inefficient to do.

## Onwards 

I have no problem writing PHP for other people. I don't become bitter and hateful when I do it. I don't start 
raging on social media that PHP sucks or anything of the sort. Because of the nostalgia element, there's even this weird 
sort of affection I have towards it. And whether you like it or not, it runs most of the web, will probably 
continue to do so because of its easy deployment story, [cheap workforce](https://survey.stackoverflow.co/2023/#technology-top-paying-technologies) and plethora of available software like content management 
systems and e-commerce systems, both categories in which PHP dominates with WordPress and WooCommerce.