---
title: "The Math of Idle Games, Part II"
source: "https://www.gamedeveloper.com/game-platforms/the-math-of-idle-games-part-ii"
author:
  - "[[Anthony Pecorella]]"
published: 2016-12-15
created: 2026-11-11
description: "Part II of a 3-part, in-depth series on the math of idle games, this time looking at alternative growth models and generator relevancy."
tags:
  - "clippings"
---
[**Featured Blog |** This community-written post highlights the best of what the game industry has to offer. Read more like it on the Game Developer Blogs.](https://www.gamedeveloper.com/keyword/blogs)

Part II of a 3-part, in-depth series on the math of idle games, this time looking at alternative growth models and generator relevancy.

![Game Developer logo in a gray background | Game Developer](https://eu-images.contentstack.com/v3/assets/blt740a130ae3c5d529/blt2c2f608b8dadcd62/6a97e133db31b5be7767f1c7/GDE27_Placeholder_-_5.png?width=1280&auto=webp&quality=80&disable=upscale "Game Developer")

This article was originally posted on [Kongregate's Developer Blog](http://developers.kongregate.com/blog).

In [Part I](http://www.kongregate.com/blog/the-math-of-idle-games-part-i) of this three-part series, we looked at some of the standard math behind rapid-growth idle games, primarily at the relationships between exponential and polynomial growth and some methods of checking and adjusting the balance of the various generators over time.

In Part II we’re going to explore a different option for growth outside of the Cookie Clicker model that the vast majority of idle games use these days.

In the "standard" model, there is a primary $\\color{ForestGreen}{currency}$ and a bunch of generators ($\\color{Cerulean}{Gen 1}$, $\\color{BurntOrange}{Gen 2}$, etc.) that produce that $\\color{ForestGreen}{currency}$. In AdVenture Capitalist these are the investments generating cash. In Clicker Heroes these are the heroes generating damage, which is then converted into gold.

But who says that generators have to produce primary $\\color{ForestGreen}{currency}$? What happens if generators generate other generators, like this?

![](https://cdn4.kongcdn.com/assets/files/0001/8770/chrome_2016-10-28_14-33-38.jpg?width=1400&auto=webp&quality=80&disable=upscale)

You have a single generator producing the $\\color{ForestGreen}{currency}$ and then a cascading chain of generators producing the previous tier. So $\\color{Cerulean}{Gen 1}$ is the rate at which $\\color{ForestGreen}{currency}$ is generated. $\\color{BurntOrange}{Gen 2}$ is the rate at which $\\color{Cerulean}{Gen 1}$ is generated, and so on. Anyone else getting twitchy calculus flashbacks? Good, because you should: these are derivatives! (Note: if you don’t know or remember calculus, don’t worry -- you’re about to learn some!)

Let’s say for a second that a single generator ($\\color{Cerulean}{Gen 1}$) produces one $\\color{ForestGreen}{currency}$ (the y-axis, representing total $\\color{ForestGreen}{currency}$) each second (the x-axis, representing total time). We’ll graph cash in green (obvi!) and you’ll see the single $\\color{Cerulean}{Gen 1}$ generator in blue. So over time the number of $\\color{Cerulean}{Gen 1}$s doesn’t change, and each second we gain 1 $\\color{ForestGreen}{currency}$.

![](https://cdn1.kongcdn.com/assets/files/0001/8773/chrome_2016-10-28_14-46-03.jpg?width=1400&auto=webp&quality=80&disable=upscale)

Now let’s say we have a single $\\color{BurntOrange}{Gen 2}$ that produces $\\color{Cerulean}{Gen 1}$s at 1 per second. What does our graph look like now?

First we need to clarify one oddity of looking at continuous graphs for a discrete problem like this. A $\\color{Cerulean}{Gen 1}$ will produce exactly 1 $\\color{ForestGreen}{currency}$ in a second. But what if that $\\color{Cerulean}{Gen 1}$ is being produced that same second? It won’t generate a full $\\color{ForestGreen}{currency}$ point but instead only half of one. You can think of it like $\\color{Cerulean}{Gen 1}$ only half-existing in the second it is created, but even if it doesn’t make total sense, if you can accept that rule then everything else lines up in that beautiful way that math always does. With that out of the way, let’s look at the magic.

At time $t$=0, we have a single $\\color{BurntOrange}{Gen 2}$ that will never change, no $\\color{Cerulean}{Gen 1}$s, and no $\\color{ForestGreen}{currency}$.

$t$=1, still a single $\\color{BurntOrange}{Gen 2}$, now a single $\\color{Cerulean}{Gen 1}$ (produced by the $\\color{BurntOrange}{Gen 2}$), and that 0.5 $\\color{ForestGreen}{currency}$ produced by that in-progress $\\color{Cerulean}{Gen 1}$.

$t$=2, we’re up to 2 $\\color{Cerulean}{Gen 1}$s, one of which generates a whole $\\color{ForestGreen}{currency}$ (the $\\color{Cerulean}{Gen 1}$ created in $t$=1) and one generates a half $\\color{ForestGreen}{currency}$. Add that to the previous half and we have two $\\color{ForestGreen}{currency}$ at $t$=2.

Now let’s graph it!

![](https://cdn1.kongcdn.com/assets/files/0001/8772/chrome_2016-10-28_14-44-19.jpg?width=1400&auto=webp&quality=80&disable=upscale)

That $\\color{ForestGreen}{currency}$ graph looks suspiciously like a parabola, no? It is in fact $y = \\frac{x^2}{2}$. For those of you who remember your integrals, that equation will look familiar as the integral of $y = x$ (which is the equation that produces the blue angled line up there). Without going too much further down a calculus lesson, here’s the summary.

A derivative is a rate of change. In this growth model, each generator represents the rate of change of the next-tier-down generator, and thus can be considered a derivative. Because these are in sequence, we can effectively keep taking integrals from a starting point to see what growth becomes as you get higher tiers of generators. The series of integrals looks like this:

$1, x, \\frac{x^2}{2}, \\frac{x^3}{6}, \\frac{x^4}{24},..., \\frac{x^n}{n!}$

So if we had 4 tiers of generators, starting with a single $Gen4$, the $\\color{ForestGreen}{currency}$ would be growing at $\\color{ForestGreen}\\frac{x^4}{24}$, and at time $t$ the total number of all generators would be $1$ + $\\color{Maroon}{t}$ + $\\color{BurntOrange}{\\frac{t^2}{2}}$ + $\\color{Cerulean}{\\frac{t^3}{6}}$.

And, just to complete our circle, we’ll take a very quick look at an example from upper-level calculus. A certain type of infinitely summed series is called a Taylor Series, and there is a specific case that’s super relevant.

$e^x = \\sum\_{n=1}^{\\infty} 1 + x + \\frac{x^2}{2} + \\frac{x^3}{6} + … + \\frac{x^n}{n!}$

In other words, as we get more and more tiers of generators (as n goes up), we approach $e^x$, which is...exponential growth!

![](https://cdn3.kongcdn.com/assets/files/0001/8742/tumblr_mhpp3g0gYM1r5rl88o1_500.gif?width=1400&auto=webp&quality=80&disable=upscale)

Okay, so that was a lot of theoretical math -- where did that actually get us? We’ve learned that setting up a chain of generators starts to approach exponential growth, which means it will have a lot of the properties that we had used in our exponential games. Plus, since you're not going to have infinite generators, you'll still always eventually lag behind actual exponential growth, so costs (at exponential levels) will still outpace production (with derivative-based growth) as we want. I also find that it just "feels" good - as you gain more tiers you get to see the lower tiers get truly huge - it's like having multiple AdVenture Capitalist games all working together!

There are a few games that already do this, perhaps most clearly is the aptly named [Derivative Clicker](http://gzgreg.github.io/DerivativeClicker/) by [gzgreg](https://github.com/gzgreg) (forked from [icehawk78](https://github.com/Icehawk78)), seen in the animated.gif below. Note that the single Graduate Student ($\\color{Maroon}{Gen 3}$) is producing one Undergrad ($\\color{BurntOrange}{Gen 2}$) per tick, while the High Schoolers ($\\color{Cerulean}{Gen 1}$, which produce dollars) being generated go up quickly -- 10, then 11, then 12, etc. as the Undergrads increase. Exactly as we set up above.

![](https://cdn4.kongcdn.com/assets/files/0001/8748/2016-10-27_14-41-14.gif?width=1400&auto=webp&quality=80&disable=upscale)

We can check out [Derivative Clicker code](https://github.com/gzgreg/DerivativeClicker/tree/gh-pages) to learn a little more, and indeed the costs are calculated using a basic exponential function as expected. The cost of High Schoolers for example is $5 \\times 1.1^n$. So still exponential costs, and growth is happening sub-exponentially, which is perfect!

Cirrial’s [Shark Game](http://cirri.al/sharks/) (a fantastic idle game if you haven’t tried it) also makes use of generators producing other generators; for example, the Nurse Sharks create Sharks that harvest fish.

One of the major balance issues to solve is how to keep purchasing of lower-level tiers relevant. As you can see in the gif above, I’m gaining lots of high schoolers every second, for free. Why would I ever buy more? The answer could be that you wouldn’t and you can design your game that way, but if you want to keep them relevant, well, gzgreg’s solution is an elegant one.

Notice that each generator up there has two numbers: a total owned, and then one in parentheses, which is the number actually purchased. The cost of a generator is calculated based on the purchased number, not the owned number. But more importantly, he has created tier boosts. For example, every purchased tier 1 building (like the High Schooler) boosts the production of all tier 1 buildings by 0.05%. This means that even when I have billions of High Schoolers, there’s still value in me being able to buy more manually, which creates a great sense of having lots of possible and influential ways to spend your resources.

One last point on the topic. What I laid out above is the simplest version of a derivative-based system, but as a game designer you have a lot more flexibility than that, and you should be open to exploring that space. For example, here’s the general cost and production chart for Derivative Clicker. Notice that it has two currencies and generators have dependency on the production of one another, which makes for some excellent interplay during the game.

![](https://cdn1.kongcdn.com/assets/files/0001/8746/Pasted_image_at_2016_10_27_05_29_PM.png?width=1400&auto=webp&quality=80&disable=upscale)

So I would encourage you to explore different types of progression within your game. The standard all-generators-contribute-to-a-primary-currency progression is great and powerful, but it can also be combined with things like this derivative-based generator concept.

Make generators. Draw lines between them. See what happens! Maybe one generator can produce multiple currencies. Or multiple generators. Or maybe even [more idle games](http://www.kongregate.com/games/kongregate/idleplex). The world’s your oyster, have fun with it.:)

If you have questions, find a mistake, or have an awesome new idle game you want to share, please feel free to reach out and email me at [anthony@kongregate.com](mailto:anthony@kongregate.com). I also want to give credit to the tools used in this post: [MathJax](https://www.mathjax.org/) did the awesome live-rendering of LaTeX, [Draw.io](https://www.draw.io/) is a quick and free flowchart web app, and [Desmos](https://www.desmos.com/) is a slick in-browser graphing calculator with some great educational functionality.

![](https://cdn4.kongcdn.com/assets/files/0001/9181/anthony_blog_nobg.png?width=1400&auto=webp&quality=80&disable=upscale)

#### Anthony Pecorella

Anthony has been at Kongregate for 8 years and is the director of virtual goods for browser games. He works to bring great games to the platform and help developers make a living with their games. He was the lead producer for the mobile version of AdVenture Capitalist.

Anthony also designs and develops games on the side with his indie game studio Level Up Labs, including the acclaimed tower defense RPG [Defender's Quest: Valley of the Forgotten](http://www.kongregate.com/games/LevelUpLabs/defenders-quest-prelude), and a MacArthur-grant-funded educational biology game, [CellCraft](http://www.kongregate.com/games/CellCraft/cellcraft).

Read more about: