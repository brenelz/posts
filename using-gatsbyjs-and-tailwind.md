---
layout: ../../layouts/PostLayout.astro
title: "Using GatsbyJS and Tailwind CSS"
date: "2019-11-11"
path: "/posts/using-gatsbyjs-and-tailwind"
---

Lately I've been intrigued by this thing called the <strong>JAMstack</strong>. It stands for JavaScript, APIs, and Markup.

## GatsbyJS

The particular technology that sticks out to me here is <a href="https://www.gatsbyjs.com/">GatsbyJS</a>. It allows you to create blazing fast websites that when it comes to shipping is just a static site. It gives you all the power of <a href="https://reactjs.org/">React</a> with a lot of boilerplate abstracted away from you.

No need to know webpack or routing as it provides you a basic framework to allow you to just start building websites. I think this is how I would build websites if I was still in the client services business.

The docs are great and they have so many plugins available to you its crazy. I like how it is CMS agnostic and allows you to use Contentful, WordPress, or even just plain old markdown files (which is what I'm using for this website).

I took the task of redeveloping my website using GatsbyJS and couldn't believe how quickly I could do it. All this while still being a newbie to it.

## Tailwind CSS

The other technology that interests me is utility frameworks and in particular Tailwind CSS. It was created by someone I have followed for a long time, <a href="https://twitter.com/adamwathan">Adam Wathan</a> so that definitely helps.

It takes a bit of a controversial approach where all you do is assign long list of class names to elements that control the styling. This is a large shift from the concept of not having style information in your markup.

When redoing my site I used Tailwind CSS and I was surprised at how my workflow changed. It was easy to play around with how I wanted padding and such. Try `px-4`, nope thats too wide, try `px-2`.
