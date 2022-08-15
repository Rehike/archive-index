# How to: YouTube JS Guide

Actually, it may be surprising to say that YouTube straight up never gave a shit about truly obfuscating their code. Their HTML is returned from the server, with even template engine artifacts being evident, their CSS is hardly minified at all, and their ActionScript code doesn't quite get mangled either.

Only their client side program code gets obfuscated, and that's usually by the compilers that they use rather than anything else.

If you've tried reading any of YouTube's JavaScript code, it is evidently very difficult to read. This is because YouTube uses Closure Compiler to optimise their JS assets so each download is more efficient.

They didn't always do this, however, before 2009 their source code was completely open, with symbol names and comments available for all to see.

If you ever want to read through YouTube's JS, you will need a fair understanding of JS and how it works. It's not as easy as reconstructing the HTML may be.

## Closure Decompiler

There is no such thing. In order to optimise sending JS over the network, Closure renames all symbols to a series of letters and removes comments and whitespace. This effectively also obfuscates the code, making it very difficult to figure out what things do what.

There is no way to create nothing from something. You will need to redocument it by hand.

(That is, unless one day the debug compilations leak, which wouldn't be surprising given they probably do exist on s.ytimg.com)

A few Closure patterns to look out for:

- `!1` and `!0`: These are small ways to create a *boolean* type in JavaScript. Using `0` and `1`, and relying on the boolean cast would be way more prone to breaking code. So just remember that these two, respectively, represent `false` and `true` precisely.
- `(expr)&&(expr)`: This is a very common and quite frankly difficult to read way of simplying if statements. Take for example this statement:
    ```js
    if (a && 0 == b)
    {
        c = handleA(a);
    }
    ```
    since everything in this block of code is an expression, it can be morphed into this:
    ```js
    (a&&0==b)&&(c=handleA(a))
    ```
    while maintaining perfect functionality.
- `(expr)||(expr)`: The opposite of the above. This inverts the left condition (the if).

Finally, Closure doesn't really have any noticeable artifacts when files are included. So if the goal of reading the JS for you is reverse engineering into a readable codebase, you will have to come up with the file delineations yourself for the most part.

## UIX

Prior to the Kevlar frontend (which uses forked Paper components and other Polymer-based components), YouTube's main component library is their in house UIX library.

Interestingly, [UIX has been documented publicly to some extent](http://velocity.oreilly.com.cn/2010/ppts/Front-endPerformanceImprovementsatYouTube.pdf). Still, the documentation remains fairly vague and unrepresentative of YouTube's own implementation.

Basically, the simplest way that I can put it is: UIX is the library that controls *widgets*, which are all elements on the page that have functionality.

UIX is really cool because it implements generally client side widgets with minimal client side rendering. For example, clickcards (popups that appear when you click on an element) are implemented as a hidden child of the clickcard container, of which the button or whatever is also a part.

For many basic types of functionality, they are implemented as UIX widgets. The compiled UIX source code is also the hardest shit to navigate on the planet, so get ready if you ever need to go through it.

## What core are you using?

Around 2013, YouTube started switching to a modularised JS core.

That is, instead of the previous `www-core.js` that contains all the source code for everything at once, the source code was broken up into `www/base.js` with additional JS modules that can be loaded and unloaded as needed.

This new design makes things easier on the network, but harder on us. Now we have multiple different sources to look through, sometimes with duplicate code (i.e. in the case of `www/pageframe.js`).

## And to finish

Remember, singlestepping is your friend.

It can be very difficult to read through their source code, simply for the lack of any meaningful variable names. It's good to singlestep and take notes on the inputs, that way you can understand the source code better.