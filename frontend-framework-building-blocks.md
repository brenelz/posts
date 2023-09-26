---
title: "Frontend Framework Building Blocks"
date: "2023-09-25"
slug: "frontend-framework-building-blocks"
---

So as many of you know I have been thrust into working with <a href="https://angular.io/">Angular</a> the last six months or so. I feel that at this point I have a intuition for what I like and don't like. This is my attempt at verbalizing why I tend to like <a href="https://react.dev/">React</a> and more recently <a href="https://www.solidjs.com/">Solid</a> over frameworks like Angular.

Now I understand that bad code and good code can be written in any framework but I feel frameworks can push you in good default directions. This is where I think Solid outshines something like Angular.

The following four points highlight the main concepts that I think differentiates them:
### JSX

Starting with maybe the most controversial. I personally find JSX good for my mental model. I much prefer it over things like `*ngFor` and `v-for` directives. I read the following in an article recently and it resonated with me.

> Templates use a more html like syntax, while JSX is more JavaScript like 

I personally like that <strong>JSX is more JavaScript like</strong>.

This leads into the next point which is I feel JSX allows <strong>better TypeScript integration</strong>. Instead of having magic strings like: `*ngIf="showCollapse; else nonCollapse` you do things like ternaries or use a `<Show>` component like Solid. I feel like component props are strongly typed and provide autocomplete, where in Angular they aren't. There are some settings and an Angular extension that can do some of the job but it just isn't the same.

Another thing the drives me crazy is the fact you have to add components to an `NgModule`. Now I think in newer Angular versions they have this concept of a <strong>standalone</strong> component which allow us to just import components and use them without fuss.

Lastly I like having my logic and template together in the same file. This is another controversial opinion but I just want <strong>less file switching, and just less files</strong> in general. I also think this way of slicing components promotes having smaller components. I have quickly seen `*ngIf` and templates get complicated very quickly.

### Read/Write Segregation

Lately I've come to appreciate Read/Write Segregation with state primitives. This is an idea I am still trying to understand deeper but this is my take. Solid's `createSignal` and React's `useState` are perfect examples of this. They both return a tuple of a getter and a setter. It allows you to be more explicit about what things can set the value. This promotes the related concept of <strong>unidirectional data flow</strong>. 

Also preferably you control how setters are called instead of providing direct access. An example is passing an `increment` function instead of the `setCounter` directly.

A good quote I found around this is as follows:

> Read/write segregation creates the opportunity to only use mindful, “local” mutability. The Accessor is designed to be «shared»; the Setter is designed to be «owned».

Along with this is a related concept of keeping your state scoped as low as possible to avoid global state. I've come to the realization that these state primitives should be defined within a component where possible. This way the setter can only affect the current component and down the tree.

Having setters that can be called from multiple different random places in the codebase is a mess. This helps keep the "locality of thinking" as <a href="https://twitter.com/RyanCarniato">Ryan Carniato</a> says.

### Functions Over Classes

Another key concept I've come to appreciate is functions over classes, or similarly composition over inheritance.

My brain just aligns a bit better with a functional programming paradigm. This paradigm tries to keep things pure as much as possible in that the same inputs get the same output.

The composability of using props and children (React/Solid) is is much greater than dependency injection in frameworks like Angular.

Functions also have less ceremony and boilerplate around them which make them easier to use, especially together with modules.

This is another obvious distinction for me between React/Angular. Angular relies heavily on classes, and almost every other framework relies on functions.

Now obviously I come more from a web background, so maybe my opinion would be different if I was coming from `C#` or `Java`.

> One of the important things I wanted to do with Solid's design was to keep the locality of thinking - Ryan Carniato

### Signals

So I won't get too much into what signals are but instead talk about the benefits of using them. You can read <a href="https://dev.to/this-is-learning/react-vs-signals-10-years-later-3k71">this article</a> if you want a bit more in depth reading.

The most talked about reason for them is the <strong>peformance</strong>. They allow frameworks to make targeted updates to the DOM instead of relying on a vdom and re-rendering everything. This is why Solid has been at the top of JS benchmarks for years.

The other benefit is the auto-tracking of dependencies in effects. This means no dependency array footguns that something like `useEffect` in React give you.

When comparing with Observables in Angular I find that most of the time I want simple synchronous reactivity. I just want to be able to get a value, and set a value in an predictable way. Most of the time I don't care about the "values over time" aspect of observables.

These are just a couple of the biggest difference between React and Solid and why Signals are influencing every framework these days including Svelte, and Angular.

> React's true strengths are: composition, unidirectional data flow, freedom from DSLs, explicit mutation, and static mental model - Dan Abramov

## Framework Comparison

So just as a quick comparison you can see below the list of frameworks and which principles they follow. This also happens to be my preference of frontend frameworks from left to right.

<p>
    <img width="500" src="/images/posts/framework-compare.png" alt="Framework Comparison">
</p>