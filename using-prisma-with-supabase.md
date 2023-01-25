---
title: "Using Prisma with Supabase"
date: "2021-06-05"
slug: "using-prisma-with-supabase"
---

So a couple of my favourite tools are
<a href="https://www.prisma.io/">Prisma</a> and
<a href="https://supabase.io/">Supabase</a>. I have talked about Supabase in
previous posts but not so much about Prisma. Prisma is a Node.js and Typescript
ORM. It makes CRUD actions on your db super easy with a familiar syntax.

In previous posts I have used the Supabase client library but in some cases you
might want to just use it as a postgresql db and do your own manual queries
using Prisma.

Below I walk through a simple app that retrieves countries from the db.

## Step 1: Setup Supabase

The first step is to <strong>create a supabase project</strong>.

From there you will need to head on over to the <strong>sql tab</strong> and
scroll down to quick start. There you will select the countries quick start and
hit the run button.

Next go to <strong>database tab</strong> and under settings go to connection
pooling. Here you will want to copy the connection string.

## Step 2: Create Nextjs Project

Run the following command in the terminal to create a new project.

```
npx create-next-app prisma-supabase
```

The only real clean up will will do for now is to open up `pages/index.js` and
remove everything so it looks like this:

```
export default function Home() {
  return <div>Content here</div>;
}
```

## Step 3: Install/Setup Prisma

Then run the following commands in your terminal to start setting up Prisma.

```
npm install prisma --save-dev
npx prisma init
```

Next change `DATABASE_URL` in the `.env` file to the connection string you
copied earlier.

Now that we have prisma setup you will see a `prisma/schema.prisma` created for
you. You could manually create models in here but there is an easier way. We can
pull in our database schema created in Supabase automatically using the
following command:

```
npx prisma introspect
```

Then we can install the Prisma client. This is neat because it actually creates
Typescript types for your models that you can use. A big feature of Prisma is
the type safety. You can get nice autocomplete for your table columns.

```
npm install @prisma/client
npx prisma generate
```

## Step 4: Use Prisma Client

Next create a file `utils/initPrisma.js` with the following contents:

```
import { PrismaClient } from "@prisma/client";
const prisma = new PrismaClient();
export default prisma;
```

Now we can use the created PrismaClient by exporting a function called
`getServerSideProps` above the existing default export:

```
import prisma from "../utils/initPrisma";

export async function getServerSideProps() {
  const countries = await prisma.countries.findMany();

  const allCountries = countries.map((country) => ({
    ...country,
    id: country.id.toString(),
  }));

  return {
    props: {
      allCountries,
    },
  };
}
```

If you are used to nextjs this should look fairly familiar returning props like
this. The weird looking part is we have to change the country.id to a string in
order to be able to serialize it properly.

## Step 5: Display Data In React

Next we can then update the default export to show this data:

```
export default function Home({ allCountries }) {
  return (
    <div>
      <ul>
        {allCountries.map((country) => (
          <li>{country.name}</li>
        ))}
      </ul>
    </div>
  );
}
```

As you can see the props we returned from `getServerSideProps` comes in here and
we can display it however we want. In this case it's just a simple unordered
list.

## Step 6: Run Server

Then start up your server with `npm run dev` and go to
<a href="http://localhost:3000/"> `http://localhost:3000/`</a> to see your
masterpiece.

## Conclusion

I haven't quite figured out when I would reach for Prisma. As I am thinking
about it I think I would use Prisma on server rendered routes
(getServerSideProps in this case) and the Supabase client library for client
side data fetching. I am not 100% set on that yet and my opinions are evolving.

I know at this point in time my rapid application development stack looks
something like the following:

&bull; Nextjs React Framework (Remix Run is making a run here)<br /> &bull;
TailwindCSS<br /> &bull; Supabase (DB, Auth, Storage)<br /> &bull; Prisma
(ORM)<br /> &bull; Sanity (CMS)<br /> &bull; Netlify (Hosting, Serverless
functions)<br />
