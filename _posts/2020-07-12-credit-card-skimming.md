---
layout: post
title: 'Online Credit Card Skimming'
subtitle: Stealing credit cards without hacking the database
tags: [software engineering, security]
---

You go online to a website and you want to buy something from it. It's the website
of a big compant. You might also see "secure payment by" followed by the name of
a payments processor. They also might be, and should be, PCI-DSS-compliant. You
feel safe, you proceed with the payment. Few months later, a data breach is
announced. What happened? How could it happen? This is exactly what we'll talk
about in this post.

## Credit Card Skimming History
Any new technology comes with its new problems. Credit cards are not an exception.
Ever since credit card readers were invented, a parallel development to them was
credit card skimmers. A credit card skimmer is essentially a card reader which
also stores the information of the card. The attacker planted a skimmer, then
came back to the collect it and make copies of those credit cards. This isn't
really history, there're also [some recent stories](https://www.msn.com/en-us/news/crime/2-arrested-for-placing-credit-card-skimmers-at-a-boca-raton-gas-station/ar-BB16CoEt) of this.

## On the Internet
Ok, we get how a credit card skimmer works against physical cards, but what does
this have to do with online payments? A lot. Hackers adopted the same idea to
steal credit cards. You see, a hacker might spend a lot of time trying to gain
access to a database where payments details are stored. Then they might find only
masked numbers and/or encrypted data. The effort and risk to reward is quite
high. Some hackers, however, were smarter than that. They made their attacks
front-end focused and targeted the web pages served to users.

The idea is quite simple, the page is going to execute some JavaScript code, so
let's put our malicious code as part of it. What made this worse was the increasing
reliance on third-party scripts from external sources. Those pieces of code are
generally very small and buried within large scripts. When the browser executes
those parts of the scripts, they read the values users entered and send them
somewhere else. This is, of course, easier said than done. The challenge now is
how to add those pieces of code in the first place.

## Recent Incidents
Perhaps the two most well-known examples of such attacks are the Magento attack
attack and the breach of British Airways. The Magento one, known as Magecarts,
targeted older version of Magento, a popular platform for building e-commerce
websites. As it turned out, there were thousands of websites using older versions
of Magento and were vulnerable to the attack. You can read more about in this
[article](https://www.darkreading.com/attacks-breaches/magecart-plants-card-skimmers-via-old-magento-plugin-flaw-/d/d-id/1337861).

The second attack was on British Airways where over 500,000 customers had their
data stolen. That's a lot of people. Even the lowest estimate provided by BA
initially, which was 380,000, is still huge. The attack followed a very similar
approach to that of Magecart, but injected the code as part of a JavaScript
library called Modernizr. This was very smart for two reasons: Modernizr will most
likely be loaded on all pages including the checkout page, and it's an external
library and its code wouldn't be examined or modified by the developers working
on the website.

## Hackers Always Catch Up
No matter what you do, and how advanced your security and protection get, hackers
will always catch up to you. This isn't to discourage you from securing whatever
it's you're building and following best practices. However, you should keep in
mind that you're not making your completely impervious to attacks but _you're
reducing the scope of who can successfully attack you_.
