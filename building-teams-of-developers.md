---
layout: ../../layouts/PostLayout.astro
title: "Building Teams Of Developers"
date: "2020-07-28"
path: "/posts/building-teams-of-developers"
---

Recently someone on twitter had some questions about building and scaling teams of developers. I figured I would try and summarize my thoughts on the matter. This has evolved through my many experiences and I still don't have all the answers. I find this stuff to be an interesting challenge as opposed to coding itself.

## What is the size of a team?

A team is kind of a loose term as in there is often teams within teams. I think ideally a team would be no bigger than 8-10 people. Above this things get kinda tough to plan and coordinate. At this point it is probably a good idea to break into sub teams.

## Who is on the team?

A team ideally has a product manager, QA, designer, developers, and tech lead. You would also have a team lead and engineering manager above this.

## Who plans the work?

Planning the work is somewhat of a team effort but falls a bit more upon the tech lead and product manager. The product manager is constantly grooming the backlog and prioritizing what the next tasks will be. Alongside this the tech lead determines exactly how the work gets done. This is delegating tasks to specific people and providing technical direction to all its members.

## How do you handle releases?

Releasing is something everybody on your team should be familiar with. Some members might need a second set of eyes on things, but everyone should own the code from the time they start a task until it is in production. You don't want to have a gatekeeper that everything must go through as this just bottlenecks everything. This is something I had to learn the hard way.

Doing frequent releases means that deploying to production should be an easy and automated task. Deploying frequently is a great indicator as to how well your company is doing.

You may think this sounds chaotic, which I might have agreed with you a few years ago, but I've come around to smaller frequent releases. This helps me avoid things like messy merge conflicts, and having to hold back merges for unreleased work. My goal is to never be very far away from deploying. As you can imagine this is tremendous for the business as you can be a bit more agile.

## How do you maintain quality with frequent releases?

The next question you would have is how do you make sure you're not releasing more and more bugs into production. Of course you have QA to catch some of it, but if you are only relying on them you are once again bottlenecked. In this case more automation is the answer. You will want to write unit/integration tests as your writing code, as well as spending time creating scenarios for your end to end tests that run in your pipeline after a deploy.

## How do you deal with ad-hoc requests?

Getting requests that aren't in your cycle of work is pretty common. What you want to do is keep these requests in a public channel, and out of private dm's. You want the ability for anyone on your team to pick these up when they have a free cycle. You again don't want one person doing all the work. Creating tickets in your project management system is also a requirement. You want to make work as visible as you can. I used to be pretty horrible at making work visible as I considered updating my tickets to not be important. I now realize to keep yourself sane you want to make it clear what you are working on so if priorities shift you aren't just taking on more and more.

## Resources

&bull; <a href="https://www.oreilly.com/library/view/accelerate/9781457191435/">Accelerate</a><br />
&bull; <a href="https://basecamp.com/shapeup/webbook">Shape Up - Basecamp</a><br />
&bull; <a href="https://www.amazon.ca/Phoenix-Project-DevOps-Helping-Business/dp/0988262592">Phoenix Project</a>
