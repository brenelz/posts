---
title: "The Power Of Netlify"
date: "2020-08-05"
path: "/posts/the-power-of-netlify"
---

<a href="https://netlify.com/">Netlify</a> is truly an amazing piece of software, as I don't think it gets any easier to host a website. Now you may think of Netlify as only a website host but it has many other interesting features that I will outline below.

As a matter of fact, my website that you are visiting right now is a static site that is deployed through Netlify. This has made publishing blog posts a snap as I only have to write markdown and then git push it and Netlify does the rest.

The following are some of the neat features Netlify provides:

## Git Based Deploys

Do you remember back in the day having to FTP your code up to a server? Common problems arose such as which files changed, how do you backup files, and how do you do a zero-downtime deploy.

These days most projects are using git (if you aren't you should be). The cool thing that Netlify provides is that when you push to your master branch it will automatically build and deploy your source code.

Another cool thing is that pull requests from other branches also gets deployed, and you even get a screenshot placed inline. This makes things super slick to send to coworkers to preview or to make sure your site isn't busted.

I see Netlify for more of a use case when building out client websites, as opposed to full-fledged products where things are more dynamic. This is changing rather rapidly though as frameworks like <a href="https://nextjs.org/">Next.js</a>, <a href="https://redwoodjs.com/">RedwoodJS</a>, and <a href="https://blitzjs.com/">BlitzJS</a> are bringing more of the full-stack to the JAMstack.

The other cool thing that this allows you to do is have instant rollbacks of deploys. You can just scroll through past deploys and hit deploy again to reset it to the previous state. No complex git reverts, or retagging docker images needed.

## Serverless Functions

Serverless functions is something that has grown in popularity in the JAMstack. It used to be thought that only static sites could be hosted on places like Netlify, but functions have kind of changed that. You can use functions to pull data from an api, send an email, connect with stripe etc... There is even a cool website that has pre-built functions for Netlify (<a href="https://jamstackfns.com">jamstackfns.com</a>)

<a href="https://docs.netlify.com/functions/build-with-javascript/#format">Netlify functions</a> are a wrapper on top of AWS Lambda that takes away a ton of configuration for you and things just work. It's also cool that they support JavaScript as well as Go.

Basically all you need to get this to work is two things. One is to make a `netlify.toml` with the following contents:

```
[build]
  functions = "functions/"
```

Then you create that folder and include a `hello.js` file as below:

```
exports.handler = function(event, context, callback) {
    callback(null, {
        statusCode: 200,
        body: "Hello, World"
    });
}
```

Then boom you commit and push your branch and Netlify automatically will setup your serverless function which can be accessed at `/.netlify/functions/hello`. How cool is that?!

## Netlify Identity

Another cool thing that comes with Netlify is a way to have a signup and login form on your website. If you've ever set this up manually you know it's challenging and can take a lot of time. You have to also have to deal with security of passwords and stuff.

Now for more powerful needs you have something like <a href="https://auth0.com/">Auth0</a> but for your basic needs the Netlify identity widget is great. To figure out how to implement it I would suggest reading the <a href="https://docs.netlify.com/visitor-access/identity/#enable-identity-in-the-ui">docs</a>.

## Netlify Forms

Lastly is Netlify Forms. Have you ever built a static site just to find out you need something dynamic to email you from your contact form? I have and its annoying. Now you could implement this yourself using serverless functions, but Netlify has made it even easier.

All you really have to do is add a `netlify` attribute to your form and deploy it. It also comes with spam filtering and other nice to haves.

## Conclusion

The two most popular JAMstack hosts that I know of are Netlify and Vercel. You may ask what the difference is between the two. The git based deploys feature is pretty much the same across both, but Netlify has a few extras like identity, and forms. Play around with both of them and let me know what you think.
