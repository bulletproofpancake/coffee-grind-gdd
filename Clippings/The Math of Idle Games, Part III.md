---
title: "The Math of Idle Games, Part III"
source: "https://www.gamedeveloper.com/design/the-math-of-idle-games-part-iii"
author:
  - "[[Anthony Pecorella]]"
published: 2017-02-01
created: 2026-11-12
description: "The final part of a 3-part, in-depth series on the math of idle games, this time looking at prestige loops and patterns."
tags:
  - "clippings"
---
[**Featured Blog |** This community-written post highlights the best of what the game industry has to offer. Read more like it on the Game Developer Blogs.](https://www.gamedeveloper.com/keyword/blogs)

The final part of a 3-part, in-depth series on the math of idle games, this time looking at prestige loops and patterns.

![Game Developer logo in a gray background | Game Developer](https://eu-images.contentstack.com/v3/assets/blt740a130ae3c5d529/blt2c2f608b8dadcd62/6a97e133db31b5be7767f1c7/GDE27_Placeholder_-_5.png?width=1280&auto=webp&quality=80&disable=upscale "Game Developer")

This three-part series is intended to break apart my [MIGS 2016 slides](https://www.slideshare.net/AnthonyPecorella/the-rise-and-rise-of-idle-games-68916528) into what are hopefully detailed but accessible angles on the math and mechanics behind idle and incremental games. The sections draw heavily from a [collection of spreadsheet models](https://kon.gg/idle-math-spreadsheets) I built and have opened up for you to play around with.

In [Part I](https://www.kongregate.com/blog/the-math-of-idle-games-part-i) we looked at some of the standard math behind rapid-growth idle games, primarily at the relationships between exponential and polynomial growth and some methods of checking and adjusting the balance of the various generators over time.

In [Part II](https://www.kongregate.com/blog/the-math-of-idle-games-part-ii) we explored different options for growth outside of the Cookie Clicker exponential model, as well as other options for interactions of generators.

Now, in the final part, we will look at prestige loops, including analyzing the design behind real-world examples and how to vary them to keep things interesting for the player.

The ability to reset your game with a multiplier to progress, often called a “prestige,” is one of the crucial mechanics behind most modern idle games, originally popularized (and conceived?) by Orteil’s Cookie Clicker. At a high level, these systems serve two primary purposes:

1) They create the “ladder climbing” effect that contributes to why idle games are so compelling. It gives players the ability to reset with a huge boost that gives a sense of power and progres. This is similar to how “launch games” (like [Burrito Bison: Launcha Libre](https://www.kongregate.com/games/JuicyBeast/burrito-bison-launcha-libre) or [Curl Up and Fly](https://www.kongregate.com/games/Kongregate/curl-up-and-fly)) have the player go as far as possible, then upgrade, then launch again.

2) They rein growth back into a more manageable number. This can give you as a developer a more stable number to key progress and upgrades off of, since growth here will be slower.

Note: I have incorrectly called this “taking a log” in the past. Most prestiges are a fractional exponent (e.g. a square or cube root) rather than a mathematical log function, so growth is still very large in some cases. Clicker Heroes is an exception that really does have a log-type effect. That said, some of these games have since added systems that do substantially rein in numbers, such as Realm Grinder’s Reincarnations and AdVenture Capitalist’s Mega Bucks.

The amount of prestige currency generated can be calculated off a variety of factors, including (but not limited to):

- Max earnings (Realm Grinder)
- Earnings since last reset (Egg, Inc.)
- Number of upgrades purchased since last reset (Clicker Heroes)

These fall into two primary categories: lifetime stats and since-reset. This fundamental difference guides general reset behavior in a significant way. Let’s dig into each of these examples a bit more to see how. For these formulas, the variables will be:

$Δp$ = prestige currency to gain  
$p$ = total prestige currency  
$c\_L$ = lifetime currency  
$c\_M$ = max currency  
$c\_R$ = currency this run

First up, Realm Grinder:

$p = \\frac{\\sqrt{1 + 8 \\cdot\\frac{c\_M}{10^{12}}} - 1}{2}$

That seems like a pretty bizarre formula, but it's actually very simple. Why? The equation for the amount $c\_M$ required for $p$ is:

$c\_M=10^{12}\\cdot\\frac{p(p+1)}{2}$

You may recognize that as the summation formula. So now we need to solve for $p$. Hmm, let's see...

$p^2+p-\\frac{2c\_M}{10^{12}}=0$

Yup, that's right. You're about to use the quadratic formula. For your job/hobby. In the real world. I bet you never thought you would! Plug it in and you'll get that first equation up there. (This legitimately got me so excited that I posted about it on Facebook.)

![](https://www.smbc-comics.com/comics/1479054311-20161113.png?width=1400&auto=webp&quality=80&disable=upscale)

© Saturday Morning Breakfast Cereal (SMBC), used with permission

Since this formula is based on the square root of the max currency earned, it means that if you reset at the same point you will earn literally no more prestige currency. To double prestige currency, a player would need to earn 4 times as much as the previous run, so that gives you a rough target to keep in mind when modeling and balancing.

Next, AdVenture Capitalist:

![](https://cdn2.kongcdn.com/assets/files/0001/9390/2015-07-10_00002.jpg?width=1400&auto=webp&quality=80&disable=upscale)

$p = 150\\cdot\\sqrt{\\frac{c\_L}{10^{15}}}$

Here we similarly have a square root, but this time based on lifetime earnings. Again assuming we want to double prestige currency each run, how much more than the previous run do we need to earn? It depends a bit on your assumptions about the previous run, but it’s likely somewhere in the similar 3x - 4x range. However, a fundamental difference is that you can keep resetting at the same point and gain currency. That said, the amount of currency diminishes each time, which means that players do need to be able to advance to make appreciable gains.

How about Cookie Clicker?

![](https://cdn4.kongcdn.com/assets/files/0001/9391/Hco.png?width=1400&auto=webp&quality=80&disable=upscale)

$p = \\sqrt\[3\]{\\frac{c\_L}{10^{12}}}$

It is also based on lifetime earnings, but using a cube root, meaning to double prestige currency, a player needs to earn roughly 8 times the previous run.

On the other side we have the systems that are agnostic to previous performance -- each run is an independent event as far as the prestige currency calculation is concerned. This means that if you reset at the same point a few times in a row you will get the same amount of currency each time. This is a very different dynamic that can create ideal strategies that don’t involve much progress in the game beyond a certain point.

These types of systems must be balanced quite differently. They have very flat curves for calculating prestige currency (that is, getting farther in a run has rapidly diminished returns).

Let's look at Egg, Inc.:

![](https://cdn2.kongcdn.com/assets/files/0001/9393/chrome_2017-01-12_17-45-57.png?width=1400&auto=webp&quality=80&disable=upscale)

$\\Delta p = (\\frac{c\_R}{10^6})^{0.14}$

That’s roughly an exponent of $\\frac{1}{7}$. So where doubling in Realm Grinder would require 4 times as much as the previous run, Egg, Inc. would require 128 ($2^7$) times as much! This could be done for a variety of design reasons, but I think one of the clearest is to nudge players into more active play and reduce the influence of time on progression. Or conversely, to accelerate progress in an offline-limited system.

In the case of Egg, Inc. I believe it to be the latter. Players are limited to 2 hours of offline play by the game. By allowing players to earn equivalent currency on equivalent runs, the game minimizes the dependency of idle gains, which is important since it has created hard limits on those gains. Progress then happens in staircase tiers through the sub-prestige loop (which roughly multiplies earnings by 5x each time) and general upgrades.

Clicker Heroes, on the other hand, has no such limits to offline gains. Instead it has a very steep difficulty curve that makes progression through the main series of “hero zones” fairly slow. As a result, the ability to keep earning prestige currency at about the same point gives players the ability to progress despite a pretty substantial progress wall. The currency itself is based on the number of upgrades purchased for the generators (heroes). Because the cost of upgrades grows at an exponential rate, this does effectively “take a log” of the overall growth curve.

Pulling these all together, in the chart below we look at the relative prestige currency gains based on earnings relative to the previous run. So the $x$-axis is a multiple of the previous run, meaning $x$=1 is earning the same amount as the previous run, $x$=2 is double, etc. The $y$-axis is prestige currency, normalized to the amount currently owned. So $y$=1 would mean that you would double your prestige currency (i.e. you earn +100%). For the sake of sanity, we’re making an assumption that the previous earnings for the lifetime calculations were ¾ of the total earnings (i.e. the previous run quadrupled the lifetime earnings to that point), so lifetime earning is $\\frac{4}{3}$ of the previous run.

![](https://cdn4.kongcdn.com/assets/files/0001/9386/chrome_2017-01-12_16-08-25.png?width=1400&auto=webp&quality=80&disable=upscale)

Looking at $y$=1 you can see rough relative earnings required to double your current prestige currency. As expected, this occurs at $x$=1 for both Egg, Inc. and Clicker Heroes, since they are independent from previous runs. Both of those curves flatten out quickly, though, and are overtaken by the other three before long.

So, that was a quick look at a variety of ways to calculate prestige currency and the design implications of them. But once you settle on a method for your game (or even use a few methods if you want to get crazy with multiple prestige currencies), how do you balance it?

To try to get a bit of a handle on this I put together “yet another spreadsheet”™ to let you play around with implications of varying the parameters. It can be found in this collection of [idle game models](https://drive.google.com/drive/folders/0B-XwqDnGiT2KWjdPbzNURkhKbmM), specifically sheet 3a. This is a very simple prestige system with only a single generator, but it can help understand the dynamics at play.

One thing to consider is that in the same way that you want to make progress through a run somewhat “bumpy” with slow parts and fast parts, you likely want to have similar variation in your prestige resets too. Much of this variation comes from the interplay of multipliers/upgrades and the rate at which prestige currency is gained (as well as the player’s targets for when to reset). Predicting player behavior in this case is difficult, but you can likely capture most players by varying within reasonable ranges.

![](https://cdn4.kongcdn.com/assets/files/0001/9388/EXCEL_2017-01-12_16-34-32.png?width=1400&auto=webp&quality=80&disable=upscale)

In this chart, generated by that spreadsheet, the $y$-axis is the number of generators owned. Each time that hits 0 it’s a prestige reset. You can see bumps of rapid purchases at 300, 400, 500, etc. generators owned -- those are due to multipliers kicking in at those totals.

There’s also a fairly long prestige starting at 120 minutes. It finally breaks at 500 generators, which is when a large 16x multiplier triggers. This is a case in which the model likely would be off -- a player who just hit a big milestone is likely to keep going for a while since that’s a fast section of the game. The point being that you should not use this model as an accurate simulation but instead as a way to try to get a rough feel for how your system would behave.

So, that’s three blog posts full of equations, graphs, and spreadsheets. While I hope that those will be useful tools for anyone working on an idle game, I think the meta takeaways are:

- There’s a lot more variety possible than the two most-common models (Cookie Clicker and Clicker Heroes). In particular, think about ways that generators can interact with one another. Draw a flowchart, then mess around with the arrows.
- Balancing progression is hard. Spreadsheet models can help, but there’s a lot of iteration required and you have many tools at your disposal.
- When designing your game, determine where the “fun” is and focus on that. Is it unfolding new features? Collecting tons of achievements? Optimizing the prestige loop? The novelty of big numbers on their own is largely gone, but there are still many opportunities for surprising and delighting players.

And with that we’ll conclude this series of idle math blog posts. I hope they have been helpful, and as always please reach out if you have any questions or cool games you want to share.:)

If you would like to contact me, shoot me an email at [anthony@kongregate.com](mailto:anthony@kongregate.com). I also want to give credit to the tools used in this post: [MathJax](https://www.mathjax.org/) did the awesome live-rendering of LaTeX and [Desmos](https://www.desmos.com/) is a slick in-browser graphing calculator with some great educational functionality.

![](https://cdn4.kongcdn.com/assets/files/0001/9181/anthony_blog_nobg.png?width=1400&auto=webp&quality=80&disable=upscale)

#### Anthony Pecorella

Anthony has been at Kongregate for 8 years and is the director of virtual goods for browser games. He works to bring great games to the platform and help developers make a living with their games. He was the lead producer for the mobile version of AdVenture Capitalist.

Anthony also designs and develops games on the side with his indie game studio Level Up Labs, including the acclaimed tower defense RPG [Defender's Quest: Valley of the Forgotten](https://www.kongregate.com/games/LevelUpLabs/defenders-quest-prelude), and a MacArthur-grant-funded educational biology game, [CellCraft](https://www.kongregate.com/games/CellCraft/cellcraft).

Read more about: