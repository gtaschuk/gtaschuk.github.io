---
layout: post
title:  "Introducing Sublingual"
date:   2016-12-22 15:53:00 -0500
---

Recently, I set out to learn German.  In school, I must admit that I was never particularly good at learning languages, and gravitated much more towards technical subjects - math, computer science, physics.  I think this was less so that I was less interested in learning languages - I have always wanted to be multilingual - but moreso because the way we learn languages is quite different from how we learn a technical subject.

A technical subject relies on layers of abstractions in which comprehension of higher level knowledge is predicated on more basic fundamentals.  You must learn single variable calculus before you can learn multivariable calculus.  Language learning, however, is much more fluid of a knowledge structure.  We learn by listening and speaking, by building neural pathways that allow us to speak fluidly, and without thought.

So when I tried to learn German as an adult, I resolved not to spend my time studying long lists of vocabulary, and word charts, but simply embrace an immersion approach to the language.  I began with the wonderful app Duolingo.  It quickly immerses you in vocab through a series of lessons that amount to roughly a year or two of language classes.  Once finished, I Wanted to continue learning, in a low effort, passive way.

I began by trying in vain to comprehend movies and shows in German.  While I could understand much of it, many of the plot elements, and nuance, was lost on me, because vocabulary went over my head, and unlike a book, it was not easy to stop, and look up words.

When I turned on English subtitles, I found it too tempting to read the english subtitles, ignoring the German audio playing in the background.

So I did what any logical programmer would do, and wrote a script, to show me definitions for words that I did not know yet in the subtitles track.  As I learned these words, I added to the list, so that fewer and fewer words appeared in the track.

In English, and many other languages, 1000 words roughly equates to 95% of words used in common speech.  This amounted to roughly as many words as I had learned in Duolingo - enough speech so that the new-vocabulary in the subtitles wouldn't be incredibly intrusive, yet there would be enough words to continue learning.

Language learning became a matter of watching movies in foreign languages, and alt-tabbing to a list of words that I knew to add words - much better than going through flashcards, or pouring over grammar charts.

After realizing the usefulness of what I had built, I couldn't find the time/desire to put it online.  As a software engineer, I gravate much more toward backend, dev-ops type work, and the html/javascript required to make the service available felt enough like work that I would rather spend my time elsewhere.  But then, in a Eureka moment, I posted an ad to a freelancer website, and in a week had found a talented Italian freelancer that built a frontend, and queing system for the script I had written.  I couldn't be happier with the work done, and soon deployed it to digitalocean, where it is now hosted.

Introducing [Sublingual.io](http://sublingual.io), the home of this application.

It has become a great side project for me, in that it's easy to pull off pieces that can be done in an evening or weekend day.  Already, people from all over the world have used the service, and I look forward to seeing more users as I add more features.

Coming soon!
<ul>
<li>User accounts where you can keep track of words learned</li>
<li>Export subtitles to dfxp format - useful for netflix</li>
<li>Better parallelization of translation api usage, for faster generation</li>
</ul>

Let me know your feedback!  I hope it helps people watch movies in other languages, and in doing so gives them the ability to talk to more people!
