---
title: "Hasura GraphQL FTW"
date: "2020-07-08"
path: "/posts/hasura-graphql-ftw"
---

Have you been wanting to dive into GraphQL but haven't found the right tool?

I think I would use <a href="https://hasura.io/">Hasura</a> for rapid GraphQL development.

Hasura basically sits on top of a relational database (Postgres) which is a big plus. I tend to favour relational databases over nosql ones. I also hear MySQL support is coming soon.

So we've just talked about a database whats the the big deal anyways. Well Hasura gives you an automatic GraphQL server to query your database. Some benefits of this are easy queries for related tables, and the ability to only choose the columns you absolutely need.

Also all the resources I looked at for GraphQL required you to kind of write your own resolvers which just seemed like too big of a hassle.

An example query would be something like:

```graphql
query {
  user {
    id
    name
    bookings {
      title
      datetime
    }
  }
}
```

## Get Started

The first step is to get a Postgres database and the hasura/graphql-engine setup. The easiest way to do this is on Heroku by hitting the <a href="https://hasura.io/docs/1.0/graphql/manual/guides/deployment/heroku-quickstart.html#deploy-to-heroku">Deploy to Heroku</a> button.

I've basically just toyed around with it. I'm not totally sure how things would scale on Heroku for a large app but I guess thats a problem to solve at a later time.

### Env Variables

I wish there was some clearer docs about what to all setup when you start, but you will want to add the following variables to Heroku.

```
HASURA_GRAPHQL_ADMIN_SECRET=somesupersecretpassword
HASURA_GRAPHQL_UNAUTHORIZED_ROLE=public
```

It took me awhile to figure out how to have an unauthorized role which is something you definitely will want before you start querying your admin. At first I was just using the admin secret but that is not secure. You will want to lock things down.

## Hasura Console

Once you have this setup on heroku you will see a dashboard something like this:

<p><img src="/images/posts/hasura-console.png" alt="Hasura Console"/></p>

From here you can click on "Data" and start creating your tables. It is pretty great interface as I picked it up fairly easily. One thing to remember is to setup your foreign keys, and also setup the associated relationships for your tables.

Once the above is done you can use the GraphiQL interface to play around and query your data.

## Pain Points

My biggest pain point in actually putting a Hasura instance into action is authorization. Having user specific permissions is easy to define on a per table basis, but to set it up in your app is clunky.

I know authorization is not an easy thing, so makes sense its tricky to setup. It seems like the current approach is to use something like <a href="https://auth0.com/">Auth0</a> and hook it into Hasura.

## Conclusion

Hasura seems super cool, and it allowed me to get up and running productive quickly. No backend api to worry about as I could just start coding the frontend in React and have Hasura be the backend.

I'm sure this breaks down when you start doing more complicated things but worth tinkering around with.

## Resources

&bull; <a href="https://hasura.io/docs/1.0/graphql/manual/getting-started/index.html#get-started-from-scratch">Hasura Quickstart</a><br />
&bull; <a href="https://hasura.io/learn/graphql/hasura/introduction/">Hasura Basics</a><br />
&bull; <a href="https://hasura.io/docs/1.0/graphql/manual/auth/authentication/jwt.html">Hasura - Authentication using JWT<a><br />
&bull; <a href="https://www.youtube.com/watch?v=LOWWtldWKYA">Scott Tries Hasura - A Realtime GraphQL API Builder</a><br />
&bull; <a href="https://www.youtube.com/watch?v=2xscUCo4KVs">User auth and roles with Hasura</a>
