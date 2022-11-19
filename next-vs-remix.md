---
layout: ../../layouts/PostLayout.astro
title: Next vs Remix (Full Stack Framework Quickstart)
date: "2022-05-22"
path: "/posts/next-vs-remix"
---

<style>
    .sidebar { display: none}
</style>

## Introduction

### What is CSR, SSG, SSR?

CSR, SSG, SSR are different ways of rendering and building a website.

Client side rendering (CSR) is what we have traditionally done in the past. It is a single page app that builds assets and doesn't require a server. It renders a blank html page except for a root div that will later get hydrated into. For this reason it is not great for SEO because none of the content is there on page load. Also all its fetching is done client side to external servers.

Static site generation (SSG) has been a more recent movement. It is an approach that builds static html files at build time that can then be deployed to a cdn. All data fetching is done at build time and gets included in the built html files. No server is required in this case, and it is good for SEO because the entire page is there on load. A good thing to remember is these can still be fairly dynamic, but it will serve the same content to every user.

Server side rendering (SSR) is what we will mostly talk about today. It is your more typically old school approach of having server rendered pages that are dynamic and serve content at runtime instead of build time. It requires a server and is also good for SEO. This is what more legacy apps using Laravel, Django, etc use. The benefit of Next and Remix is you can use the same language on the frontend and backend.

### React Framework Timeline

So now that we know a bit more about the different types of rendering concepts, we can discuss how this pertains to existing React frameworks.

Probably still one of the more popular ways to get a React app running is <a href="https://create-react-app.dev/">Create React App</a>. This is a CSR type of framework where no node server is needed. It mainly sets up a boilerplate project for you to get started quickly.

Then came along <a href="https://www.gatsbyjs.com/">Gatsby</a> which popularized the SSG way of thinking along with hosting companies such as <a href="https://vercel.com/">Vercel</a> and <a href="https://www.netlify.com/">Netlify</a>. One of the biggest pros was blazing fast performance due to hosting on a cdn, and the large ecosystem of plugins it has. The plugin system is still a heavy advantage it has over competitors. The downside is it has a complicated learning curve which is why most people prefer Next or Remix.

Speaking of <a href="https://nextjs.org/">Next</a>, it helped bring a hybrid mental model to the mainstream. You could do SSG or SSR on a per page basis! Kinda mind blowing 🤯.

Then lastly <a href="https://remix.run/">Remix</a> came around. It took the approach of being SSR only which has sparked some debate. Typically it has been said the static sites will always be faster, but Remix has challenged that idea.

We will go more in depth into Next vs Remix shortly.

### Why do you need a Next or Remix?

Using Next or Remix has the following advantages:

&bull; Templated project structure<br />
&bull; File based routing<br />
&bull; Easier data fetching<br />
&bull; Simplified data writes<br />
&bull; Api routes<br />
&bull; Improved error handling<br />
&bull; Increased performance

One downside I will quickly mentioned with these more hybrid frameworks is that it blurs the line between frontend and backend. This is especially tricky for environment variables as you try not to expose them on the frontend. Also knowing when your in node and can connect to a db as opposed to a frontend component is important.

## Quickstart Compare

To give you a quick overview of some concepts from these two frameworks take a look at the following comparison.

<div style="display:flex;gap: 40px;margin-top: 20px">

<div style="flex: 1; overflow: scroll">
<p><svg height="40" style="transform:translateX(4%);shape-rendering:auto" version="1.1" viewBox="0 0 148 90" width="82" xmlns:xlink="http://www.w3.org/1999/xlink"><path d="M34.992 23.495h27.855v2.219H37.546v16.699h23.792v2.219H37.546v18.334h25.591v2.219H34.992v-41.69zm30.35 0h2.96l13.115 18.334 13.405-18.334L113.055.207 83.1 43.756l15.436 21.429H95.46L81.417 45.683 67.316 65.185h-3.018L79.85 43.756 65.343 23.495zm34.297 2.219v-2.219h31.742v2.219h-14.623v39.47h-2.554v-39.47H99.64zM.145 23.495h3.192l44.011 66.003L29.16 65.185 2.814 26.648l-.116 38.537H.145v-41.69zm130.98 38.801c-.523 0-.914-.405-.914-.928 0-.524.391-.929.913-.929.528 0 .913.405.913.929 0 .523-.385.928-.913.928zm2.508-2.443H135c.019.742.56 1.24 1.354 1.24.888 0 1.391-.535 1.391-1.539v-6.356h1.391v6.362c0 1.808-1.043 2.849-2.77 2.849-1.62 0-2.732-1.01-2.732-2.556zm7.322-.08h1.379c.118.853.95 1.395 2.149 1.395 1.117 0 1.937-.58 1.937-1.377 0-.685-.521-1.097-1.708-1.377l-1.155-.28c-1.62-.38-2.36-1.166-2.36-2.487 0-1.602 1.304-2.668 3.26-2.668 1.82 0 3.15 1.066 3.23 2.58h-1.354c-.13-.828-.85-1.346-1.894-1.346-1.1 0-1.832.53-1.832 1.34 0 .642.472 1.01 1.64 1.284l.987.243c1.838.43 2.596 1.178 2.596 2.53 0 1.72-1.33 2.799-3.453 2.799-1.987 0-3.323-1.029-3.422-2.637z" fill="black" fill-rule="nonzero"></path></svg></p>

&bull; `npx create-next-app@latest --typescript`<br />
&bull; SWC for compiling/bundling<br />
&bull; Page routes in `pages`<br />
&bull; Api routes in `pages/api`<br />
&bull; `getServerSideProps` (similar to Remix loader)

</div>
<div style="flex: 1; overflow: scroll">
<p style="margin-bottom:12px"><img src="/images/posts/remix.svg" alt="Remix"/></p>

&bull; `npx create-remix@latest`<br />
&bull; Esbuild for compiling/bundling<br />
&bull; Page routes in `app/routes`<br />
&bull; Api routes in `app/routes` with only `loader`<br />
&bull; `loader`<br />
&bull; `action` (nothing similar in Next)

</div>
</div>

## Building A Web Application (side by side)

Today we will be building a todo list to introduce you to some concepts these frameworks provide. It will have the ability to list todo items, as well as delete and add. Instead of inputting the todo item yourself we will pull from the <a href="https://www.boredapi.com/api/activity">bored api</a>.

### Templated project structure

So one thing these frameworks give you is a templated project structure. They both get you started with an eslint config, typescript, and a compiler.

Below you can see the commands to bootstrap and start a dev server for each.

<div style="display:flex;gap: 40px;margin-top: 20px">

<div style="flex: 1; overflow: scroll">
<p><svg height="40" style="transform:translateX(4%);shape-rendering:auto" version="1.1" viewBox="0 0 148 90" width="82" xmlns:xlink="http://www.w3.org/1999/xlink"><path d="M34.992 23.495h27.855v2.219H37.546v16.699h23.792v2.219H37.546v18.334h25.591v2.219H34.992v-41.69zm30.35 0h2.96l13.115 18.334 13.405-18.334L113.055.207 83.1 43.756l15.436 21.429H95.46L81.417 45.683 67.316 65.185h-3.018L79.85 43.756 65.343 23.495zm34.297 2.219v-2.219h31.742v2.219h-14.623v39.47h-2.554v-39.47H99.64zM.145 23.495h3.192l44.011 66.003L29.16 65.185 2.814 26.648l-.116 38.537H.145v-41.69zm130.98 38.801c-.523 0-.914-.405-.914-.928 0-.524.391-.929.913-.929.528 0 .913.405.913.929 0 .523-.385.928-.913.928zm2.508-2.443H135c.019.742.56 1.24 1.354 1.24.888 0 1.391-.535 1.391-1.539v-6.356h1.391v6.362c0 1.808-1.043 2.849-2.77 2.849-1.62 0-2.732-1.01-2.732-2.556zm7.322-.08h1.379c.118.853.95 1.395 2.149 1.395 1.117 0 1.937-.58 1.937-1.377 0-.685-.521-1.097-1.708-1.377l-1.155-.28c-1.62-.38-2.36-1.166-2.36-2.487 0-1.602 1.304-2.668 3.26-2.668 1.82 0 3.15 1.066 3.23 2.58h-1.354c-.13-.828-.85-1.346-1.894-1.346-1.1 0-1.832.53-1.832 1.34 0 .642.472 1.01 1.64 1.284l.987.243c1.838.43 2.596 1.178 2.596 2.53 0 1.72-1.33 2.799-3.453 2.799-1.987 0-3.323-1.029-3.422-2.637z" fill="black" fill-rule="nonzero"></path></svg></p>

```
npx create-next-app@latest --typescript
cd my-app
yarn dev
```

<p><img width="250" src="/images/posts/next-structure.png" alt="Next project structure"/></p>

</div>
<div style="flex: 1; overflow: scroll">
<p style="margin-bottom:31px"><img src="/images/posts/remix.svg" alt="Remix"/></p>

```
npx create-remix@latest
cd my-app
yarn dev
```

<p><img width="250" src="/images/posts/remix-structure.png" alt="Remix project structure"/></p>

</div>
</div>

You also see a screenshot of what the project structure looks like. Most of the time you will be modifying files in the `pages` or `app` folder for each framework. They also have their specific configs (`next.config.js`, `remix.config.js`).

One thing a bit different with Remix is that it exposes the whole html document and doesn't hide anything from you. You will see within the `app` folder there is a `entry.client.tsx` and `entry.server.tsx`. More interesting however is the `root.tsx`. This functions as the layout for the entire app and even allows you to disable JavaScript! The Remix team is high on progressive enhancement so the app still works even without JavaScript. In Next layout is a bit more complex and you have to modify a special `_app.tsx` file.

### File based Routing

File based routing is a bit controversial. Some people like the simplicity of it and others prefer not having their routes map to a file. Dynamic routing is a bit tricky but it is possible using filenames like `[id].tsx` or `$id.tsx` as shown in the code snippet below.

_Note: Each of the code snippets following this has the name and path of the file commented at the top so you know where to put the code._

<div style="display:flex;gap: 40px;margin-top: 20px">

<div style="flex: 1; overflow: scroll">
<p><svg height="40" style="transform:translateX(4%);shape-rendering:auto" version="1.1" viewBox="0 0 148 90" width="82" xmlns:xlink="http://www.w3.org/1999/xlink"><path d="M34.992 23.495h27.855v2.219H37.546v16.699h23.792v2.219H37.546v18.334h25.591v2.219H34.992v-41.69zm30.35 0h2.96l13.115 18.334 13.405-18.334L113.055.207 83.1 43.756l15.436 21.429H95.46L81.417 45.683 67.316 65.185h-3.018L79.85 43.756 65.343 23.495zm34.297 2.219v-2.219h31.742v2.219h-14.623v39.47h-2.554v-39.47H99.64zM.145 23.495h3.192l44.011 66.003L29.16 65.185 2.814 26.648l-.116 38.537H.145v-41.69zm130.98 38.801c-.523 0-.914-.405-.914-.928 0-.524.391-.929.913-.929.528 0 .913.405.913.929 0 .523-.385.928-.913.928zm2.508-2.443H135c.019.742.56 1.24 1.354 1.24.888 0 1.391-.535 1.391-1.539v-6.356h1.391v6.362c0 1.808-1.043 2.849-2.77 2.849-1.62 0-2.732-1.01-2.732-2.556zm7.322-.08h1.379c.118.853.95 1.395 2.149 1.395 1.117 0 1.937-.58 1.937-1.377 0-.685-.521-1.097-1.708-1.377l-1.155-.28c-1.62-.38-2.36-1.166-2.36-2.487 0-1.602 1.304-2.668 3.26-2.668 1.82 0 3.15 1.066 3.23 2.58h-1.354c-.13-.828-.85-1.346-1.894-1.346-1.1 0-1.832.53-1.832 1.34 0 .642.472 1.01 1.64 1.284l.987.243c1.838.43 2.596 1.178 2.596 2.53 0 1.72-1.33 2.799-3.453 2.799-1.987 0-3.323-1.029-3.422-2.637z" fill="black" fill-rule="nonzero"></path></svg></p>

```
// pages/todos.tsx

import TodoDetails from "../components/TodoDetails";
import TodoLayout from "../layouts/TodoLayout";

export default function Todos() {
  return (
    <TodoLayout>
      <TodoDetails />
    </TodoLayout>
  );
}
```

```
// layouts/TodoLayout.tsx
import Link from "next/link";
import { ReactNode } from "react";

type TodoLayoutProps = {
  children: ReactNode;
};

export default function TodoLayout({ children }: TodoLayoutProps) {
  return (
    <div>
      <h1>Todos</h1>
      <div className="header">
        <ul>
          <li>
            <Link href="/todos/1">Item 1</Link>
          </li>
          <li>
            <Link href="/todos/2">Item 2</Link>
          </li>
          <li>
            <Link href="/todos/3">Item 3</Link>
          </li>
        </ul>
      </div>
      {children}
    </div>
  );
}
```

```
// components/TodoDetails.tsx

export default function TodosDetails() {
  return (
    <div>
      <h2>Pick an Item</h2>
    </div>
  );
}
```

```
// pages/todos/[id].tsx

import { useRouter } from "next/router";
import TodoLayout from "../../layouts/TodoLayout";

export default function TodosDetails() {
  const router = useRouter();
  return (
    <TodoLayout>
      <h2>Item {router.query.id}</h2>
    </TodoLayout>
  );
}
```

</div>
<div style="flex: 1; overflow: scroll">
<p style="margin-bottom:31px"><img src="/images/posts/remix.svg" alt="Remix"/></p>

```
// app/routes/todos.tsx

import { Outlet } from "@remix-run/react";
import { Link } from "remix";

export default function Index() {
  return (
    <div>
      <h1>Todos</h1>
      <div className="header">
        <ul>
          <li>
            <Link to="/todos/1">Item 1</Link>
          </li>
          <li>
            <Link to="/todos/2">Item 2</Link>
          </li>
          <li>
            <Link to="/todos/3">Item 3</Link>
          </li>
        </ul>
      </div>
      <Outlet />
    </div>
  );
}
```

```
// app/routes/todos/index.tsx

export default function TodosIndex() {
  return (
    <div>
      <h2>Pick an Item</h2>
    </div>
  );
}
```

```
// app/routes/todos/$id.tsx

import { useParams } from "@remix-run/react";

export default function TodosDetails() {
  const params = useParams();
  return (
    <div>
      <h2>Item {params["id"]}</h2>
    </div>
  );
}
```

</div>
</div>

As you can see this particular layout is a bit easier in Remix. You only have to add three files where Next requires you add four. It isn't just the number of files but the complexity of them. Remix's secret sauce is nested routing. A parent route just defines an `<Outlet />` where its child route will end up. In Next we are required to hand bomb this a bit more ourself by wrapping our code in a layout component.

The other thing to note is how easy it is to pull the id out of the dynamic route path using `useRouter` and `useParams`.

### Easier data fetching (static)

Data fetching is at the heart of most apps. You will most likely be calling an api at some point. Client side fetching is something both of these frameworks leave up to you to handle. You can use a raw `fetch` query in a `useEffect`, or a library like React Query, SWR, or RTK Query. What I find with Remix is that client side fetching is rarely necessary. We can usually provide our data from the server using `loaders` or in Next's case `getServerSideProps`.

Let's start by getting some static data from the server.

<div style="display:flex;gap: 40px;margin-top: 20px">

<div style="flex: 1; overflow: scroll">
<p><svg height="40" style="transform:translateX(4%);shape-rendering:auto" version="1.1" viewBox="0 0 148 90" width="82" xmlns:xlink="http://www.w3.org/1999/xlink"><path d="M34.992 23.495h27.855v2.219H37.546v16.699h23.792v2.219H37.546v18.334h25.591v2.219H34.992v-41.69zm30.35 0h2.96l13.115 18.334 13.405-18.334L113.055.207 83.1 43.756l15.436 21.429H95.46L81.417 45.683 67.316 65.185h-3.018L79.85 43.756 65.343 23.495zm34.297 2.219v-2.219h31.742v2.219h-14.623v39.47h-2.554v-39.47H99.64zM.145 23.495h3.192l44.011 66.003L29.16 65.185 2.814 26.648l-.116 38.537H.145v-41.69zm130.98 38.801c-.523 0-.914-.405-.914-.928 0-.524.391-.929.913-.929.528 0 .913.405.913.929 0 .523-.385.928-.913.928zm2.508-2.443H135c.019.742.56 1.24 1.354 1.24.888 0 1.391-.535 1.391-1.539v-6.356h1.391v6.362c0 1.808-1.043 2.849-2.77 2.849-1.62 0-2.732-1.01-2.732-2.556zm7.322-.08h1.379c.118.853.95 1.395 2.149 1.395 1.117 0 1.937-.58 1.937-1.377 0-.685-.521-1.097-1.708-1.377l-1.155-.28c-1.62-.38-2.36-1.166-2.36-2.487 0-1.602 1.304-2.668 3.26-2.668 1.82 0 3.15 1.066 3.23 2.58h-1.354c-.13-.828-.85-1.346-1.894-1.346-1.1 0-1.832.53-1.832 1.34 0 .642.472 1.01 1.64 1.284l.987.243c1.838.43 2.596 1.178 2.596 2.53 0 1.72-1.33 2.799-3.453 2.799-1.987 0-3.323-1.029-3.422-2.637z" fill="black" fill-rule="nonzero"></path></svg></p>

```
// pages/todos.tsx

import TodoDetails from "../components/TodoDetails";
import TodoLayout from "../layouts/TodoLayout";

export type Todo = {
  id: number;
  title: string;
};

type TodosProps = {
  todos: Todo[];
};

export default function Todos({ todos }: TodosProps) {
  return (
    <TodoLayout todos={todos}>
      <TodoDetails />
    </TodoLayout>
  );
}

export async function getServerSideProps() {
  return {
    props: {
      todos: [
        { id: 1, title: "Item 1" },
        { id: 2, title: "Item 2" },
        { id: 3, title: "Item 3" },
      ],
    },
  };
}
```

```
// pages/layouts/TodoLayout.tsx

import Link from "next/link";
import { ReactNode } from "react";
import { Todo } from "../pages/todos";

type TodoLayoutProps = {
  children: ReactNode;
  todos: Todo[];
};

export default function TodoLayout({ children, todos }: TodoLayoutProps) {
  return (
    <div>
      <h1>Todos</h1>
      <div className="header">
        <ul>
          {todos.map((todo) => (
            <li key={todo.id}>
              <Link href={`/todos/${todo.id}`}>{todo.title}</Link>
            </li>
          ))}
        </ul>
      </div>
      {children}
    </div>
  );
}
```

```
// pages/todos/[id].tsx

import { useRouter } from "next/router";
import TodoLayout from "../../layouts/TodoLayout";
import { Todo } from "../todos";

type TodoDetailsProps = {
  todos: Todo[];
};

export default function TodosDetails({ todos }: TodoDetailsProps) {
  const router = useRouter();
  return (
    <TodoLayout todos={todos}>
      <h2>Item {router.query.id}</h2>
    </TodoLayout>
  );
}

export async function getServerSideProps() {
  return {
    props: {
      todos: [
        { id: 1, title: "Item 1" },
        { id: 2, title: "Item 2" },
        { id: 3, title: "Item 3" },
      ],
    },
  };
}
```

</div>
<div style="flex: 1; overflow: scroll">
<p style="margin-bottom:31px"><img src="/images/posts/remix.svg" alt="Remix"/></p>

```
// app/routes/todos.tsx

import { Outlet } from "@remix-run/react";
import { Link, useLoaderData } from "@remix-run/react";
import type { LoaderFunction } from "remix";

export type Todo = {
  id: number;
  title: string;
};

export const loader: LoaderFunction = async () => {
  return [
    { id: 1, title: "Item 1" },
    { id: 2, title: "Item 2" },
    { id: 3, title: "Item 3" },
  ];
};

export default function Index() {
  const todos = useLoaderData<Todo[]>();
  return (
    <div>
      <h1>Todos</h1>
      <div className="header">
        <ul>
          {todos.map((todo) => (
            <li key={todo.id}>
              <Link to={`/todos/${todo.id}`}>{todo.title}</Link>
            </li>
          ))}
        </ul>
      </div>
      <Outlet />
    </div>
  );
}
```

</div>
</div>

As you can see because of nested routing Remix has a leg up. You will notice we had to define the `getServerSideProps` function twice where in Remix we could define just one `loader`. The other thing to note is Next requires your data object be wrapped in an additional object property called `props`. As well, Next passes the data as props to the component where in Remix you call a `useLoaderData` hook.

_Note: The `loader` and `getServerSideProps` functions can only be defined in page components._

### Easier data fetching (from db)

We rarely use static data, so let's integrate with a database. The power of `getServerSideProps` and `loaders` are they are on the server and just pass data to the component at runtime. They get stripped out of the bundle completely and just run as a server that have a contract with our components. No need for an intermediate api!

Let's use <a href="https://www.prisma.io/">Prisma</a> for our ORM to connect to a SQLite db. This gets a bit more complicated as we have to install `prisma`, and `@prisma/client` but the commands below walk you through it.

<div style="display:flex;gap: 40px;margin-top: 20px">

<div style="flex: 1; overflow: scroll">
<p><svg height="40" style="transform:translateX(4%);shape-rendering:auto" version="1.1" viewBox="0 0 148 90" width="82" xmlns:xlink="http://www.w3.org/1999/xlink"><path d="M34.992 23.495h27.855v2.219H37.546v16.699h23.792v2.219H37.546v18.334h25.591v2.219H34.992v-41.69zm30.35 0h2.96l13.115 18.334 13.405-18.334L113.055.207 83.1 43.756l15.436 21.429H95.46L81.417 45.683 67.316 65.185h-3.018L79.85 43.756 65.343 23.495zm34.297 2.219v-2.219h31.742v2.219h-14.623v39.47h-2.554v-39.47H99.64zM.145 23.495h3.192l44.011 66.003L29.16 65.185 2.814 26.648l-.116 38.537H.145v-41.69zm130.98 38.801c-.523 0-.914-.405-.914-.928 0-.524.391-.929.913-.929.528 0 .913.405.913.929 0 .523-.385.928-.913.928zm2.508-2.443H135c.019.742.56 1.24 1.354 1.24.888 0 1.391-.535 1.391-1.539v-6.356h1.391v6.362c0 1.808-1.043 2.849-2.77 2.849-1.62 0-2.732-1.01-2.732-2.556zm7.322-.08h1.379c.118.853.95 1.395 2.149 1.395 1.117 0 1.937-.58 1.937-1.377 0-.685-.521-1.097-1.708-1.377l-1.155-.28c-1.62-.38-2.36-1.166-2.36-2.487 0-1.602 1.304-2.668 3.26-2.668 1.82 0 3.15 1.066 3.23 2.58h-1.354c-.13-.828-.85-1.346-1.894-1.346-1.1 0-1.832.53-1.832 1.34 0 .642.472 1.01 1.64 1.284l.987.243c1.838.43 2.596 1.178 2.596 2.53 0 1.72-1.33 2.799-3.453 2.799-1.987 0-3.323-1.029-3.422-2.637z" fill="black" fill-rule="nonzero"></path></svg></p>

```
yarn add --dev prisma
npx prisma init
```

```
// prisma/schema.prisma

generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "sqlite"
  url      = "file:./dev.db"
}

model Todo {
  id    Int    @id @default(autoincrement())
  title String
}
```

```
yarn add @prisma/client
npx prisma generate
npx prisma db push
```

```
sqlite3 prisma/dev.db

INSERT INTO todo VALUES (1, 'Item 1');
INSERT INTO todo VALUES (2, 'Item 2');
INSERT INTO todo VALUES (3, 'Item 3');
```

```
// pages/todos.tsx

import { PrismaClient } from "@prisma/client";

// ...(rest of component)

export async function getServerSideProps() {
  const prisma = new PrismaClient();
  const todos = await prisma.todo.findMany();

  return {
    props: {
      todos,
    },
  };
}

// ...(rest of component)
```

```
// pages/todos/[id].tsx
import { PrismaClient } from "@prisma/client";
import { GetServerSideProps } from "next";
// ...(rest of component)

export const getServerSideProps: GetServerSideProps = async ({ params }) => {
  const prisma = new PrismaClient();
  const todos = await prisma.todo.findMany();
  let todo = null;
  if (params?.id) {
    todo = await prisma.todo.findFirst({ where: { id: +params.id } });
  }

  return {
    props: {
      todos,
      todo,
    },
  };
};

// ...(rest of component)
```

</div>
<div style="flex: 1; overflow: scroll">
<p style="margin-bottom:31px"><img src="/images/posts/remix.svg" alt="Remix"/></p>

```
yarn add --dev prisma
npx prisma init
```

```
// prisma/schema.prisma

generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "sqlite"
  url      = "file:./dev.db"
}

model Todo {
  id    Int    @id @default(autoincrement())
  title String
}
```

```
yarn add @prisma/client
npx prisma generate
npx prisma db push
```

```
sqlite3 prisma/dev.db

INSERT INTO todo VALUES (1, 'Item 1');
INSERT INTO todo VALUES (2, 'Item 2');
INSERT INTO todo VALUES (3, 'Item 3');
```

```
// app/routes/todos.tsx

import { PrismaClient } from "@prisma/client";

export const loader: LoaderFunction = async () => {
  const prisma = new PrismaClient();

  const todos = await prisma.todo.findMany();
  return todos;
};

// ...(rest of component)

```

```
// app/routes/todos/$id.tsx

import { PrismaClient } from "@prisma/client";
import type { LoaderFunction } from "remix";
import { useLoaderData } from "@remix-run/react";
import type { Todo } from "../todos";

export const loader: LoaderFunction = async ({ params }) => {
  const prisma = new PrismaClient();

  const todo = await prisma.todo.findFirst({ where: { id: +params.id } });
  return todo;
};

export default function TodosDetails() {
  const todo = useLoaderData<Todo>();
  return (
    <div>
      <h2>{todo.title}</h2>
    </div>
  );
}
```

</div>
</div>

Above we have set up our db, and also connected to it directly from our app from `getServerSideProps` and `loaders`. Now normally you wouldn't instantiate a new instance of the `PrismaClient` every time but we will do it for now for simplicity sake. Other than that not too much new here other than Prisma specific stuff which we aren't focusing on.

### Simplified data writes (actions)

You know above that I said Remix's secret was nested routing? I think `actions` kinda fly under the radar. They simplify data writes big time, but they require a mindset shift. Remember the `<form>` and `<input type="hidden">` tags? They will become your best friend when using Remix. This abstraction reduces the use of `useState` and `useEffect` hooks as a side effect.

In the following code snippets we add delete functionality.

<div style="display:flex;gap: 40px;margin-top: 20px">

<div style="flex: 1; overflow: scroll">
<p><svg height="40" style="transform:translateX(4%);shape-rendering:auto" version="1.1" viewBox="0 0 148 90" width="82" xmlns:xlink="http://www.w3.org/1999/xlink"><path d="M34.992 23.495h27.855v2.219H37.546v16.699h23.792v2.219H37.546v18.334h25.591v2.219H34.992v-41.69zm30.35 0h2.96l13.115 18.334 13.405-18.334L113.055.207 83.1 43.756l15.436 21.429H95.46L81.417 45.683 67.316 65.185h-3.018L79.85 43.756 65.343 23.495zm34.297 2.219v-2.219h31.742v2.219h-14.623v39.47h-2.554v-39.47H99.64zM.145 23.495h3.192l44.011 66.003L29.16 65.185 2.814 26.648l-.116 38.537H.145v-41.69zm130.98 38.801c-.523 0-.914-.405-.914-.928 0-.524.391-.929.913-.929.528 0 .913.405.913.929 0 .523-.385.928-.913.928zm2.508-2.443H135c.019.742.56 1.24 1.354 1.24.888 0 1.391-.535 1.391-1.539v-6.356h1.391v6.362c0 1.808-1.043 2.849-2.77 2.849-1.62 0-2.732-1.01-2.732-2.556zm7.322-.08h1.379c.118.853.95 1.395 2.149 1.395 1.117 0 1.937-.58 1.937-1.377 0-.685-.521-1.097-1.708-1.377l-1.155-.28c-1.62-.38-2.36-1.166-2.36-2.487 0-1.602 1.304-2.668 3.26-2.668 1.82 0 3.15 1.066 3.23 2.58h-1.354c-.13-.828-.85-1.346-1.894-1.346-1.1 0-1.832.53-1.832 1.34 0 .642.472 1.01 1.64 1.284l.987.243c1.838.43 2.596 1.178 2.596 2.53 0 1.72-1.33 2.799-3.453 2.799-1.987 0-3.323-1.029-3.422-2.637z" fill="black" fill-rule="nonzero"></path></svg></p>

```
// pages/todos/[id].tsx

import { PrismaClient } from "@prisma/client";
import { GetServerSideProps } from "next";
import { FormEvent, useState } from "react";
import TodoLayout from "../../layouts/TodoLayout";
import { Todo } from "../todos";
import { useRouter } from "next/router";

type TodoDetailsProps = {
  todos: Todo[];
  todo: Todo;
};

export default function TodosDetails({ todos, todo }: TodoDetailsProps) {
  const router = useRouter();
  const [id, setId] = useState(todo.id);
  const handleSubmit = async (event: FormEvent<HTMLFormElement>) => {
    event.preventDefault();
    const res = await fetch("http://localhost:3000/api/deleteTodo", {
      method: "post",
      body: JSON.stringify({
        id,
      }),
    });

    const json = await res.json();

    if (json.success) {
      router.push("/todos");
    }
  };
  return (
    <TodoLayout todos={todos}>
      <h2>{todo.title}</h2>
      <form method="post" onSubmit={handleSubmit}>
        <input
          type="hidden"
          name="id"
          defaultValue={id}
          onChange={(e) => setId(+e.target.value)}
        />
        <button type="submit">Delete</button>
      </form>
    </TodoLayout>
  );
}

// ...(getServerSideProps function)
```

```
// pages/api/deleteTodo.ts

import { PrismaClient } from "@prisma/client";
import type { NextApiRequest, NextApiResponse } from "next";

export default async function handler(
  request: NextApiRequest,
  response: NextApiResponse
) {
  const json = JSON.parse(request.body);

  const prisma = new PrismaClient();
  await prisma.todo.delete({ where: { id: json.id } });

  return response.json({ success: true });
}
```

</div>
<div style="flex: 1; overflow: scroll">
<p style="margin-bottom:31px"><img src="/images/posts/remix.svg" alt="Remix"/></p>

```
// app/routes/todos/$id.tsx

import { PrismaClient } from "@prisma/client";
import { useLoaderData } from "@remix-run/react";
import { Form } from "@remix-run/react";
import type { Todo } from "../todos";
import type { ActionFunction, LoaderFunction } from "remix";
import { redirect } from "remix";

// ...(loader function)

export const action: ActionFunction = async ({ request }) => {
  //@ts-expect-error
  const formData = await request.formData();
  const id = +formData.get("id");

  const prisma = new PrismaClient();
  await prisma.todo.delete({ where: { id } });

  return redirect("/todos");
};

export default function TodosDetails() {
  const todo = useLoaderData<Todo>();
  return (
    <div>
      <h2>{todo.title}</h2>
      <Form method="post">
        <input type="hidden" name="id" value={todo.id} />
        <button type="submit">Delete</button>
      </Form>
    </div>
  );
}
```

</div>
</div>

On the left is what you are probably used to doing. Tracking the state of our id input with `useState`, then doing a `fetch` to an endpoint of ours. We are lucky in this case that creating this endpoint is fairly easy.

On the right its so much simpler. You define an `action` function similar to `loader` that does the deleting based on the `formData` that was submitted. The entire form is serialized and sent to the backend without any manual tracking of state. By default the `<Form>` tag calls the action of the current page, but you can use `<Form action="some-other-page">` to post to a different action. It is also neat that Remix can tell what parts of your app have been updated by this `action` and will update the ui accordingly. No need to invalide caches manually!

### Improved error handling

Inevitably we run into errors in our app. I'm sure we have all come across the white screen of death due to an undefined variable at some point in our JavaScript career. Frameworks like these handle these errors more gracefully. In Remix's case we can just export an `ErrorBoundary` and just that one nested part of our app will show an error while parent routes will function normally. In Next things are more complicated so I would suggest you read up on <a href="https://nextjs.org/docs/advanced-features/error-handling">error boundaries</a> in the docs

<div style="display:flex;gap: 40px;margin-top: 20px">

<div style="flex: 1; overflow: scroll">
<p><svg height="40" style="transform:translateX(4%);shape-rendering:auto" version="1.1" viewBox="0 0 148 90" width="82" xmlns:xlink="http://www.w3.org/1999/xlink"><path d="M34.992 23.495h27.855v2.219H37.546v16.699h23.792v2.219H37.546v18.334h25.591v2.219H34.992v-41.69zm30.35 0h2.96l13.115 18.334 13.405-18.334L113.055.207 83.1 43.756l15.436 21.429H95.46L81.417 45.683 67.316 65.185h-3.018L79.85 43.756 65.343 23.495zm34.297 2.219v-2.219h31.742v2.219h-14.623v39.47h-2.554v-39.47H99.64zM.145 23.495h3.192l44.011 66.003L29.16 65.185 2.814 26.648l-.116 38.537H.145v-41.69zm130.98 38.801c-.523 0-.914-.405-.914-.928 0-.524.391-.929.913-.929.528 0 .913.405.913.929 0 .523-.385.928-.913.928zm2.508-2.443H135c.019.742.56 1.24 1.354 1.24.888 0 1.391-.535 1.391-1.539v-6.356h1.391v6.362c0 1.808-1.043 2.849-2.77 2.849-1.62 0-2.732-1.01-2.732-2.556zm7.322-.08h1.379c.118.853.95 1.395 2.149 1.395 1.117 0 1.937-.58 1.937-1.377 0-.685-.521-1.097-1.708-1.377l-1.155-.28c-1.62-.38-2.36-1.166-2.36-2.487 0-1.602 1.304-2.668 3.26-2.668 1.82 0 3.15 1.066 3.23 2.58h-1.354c-.13-.828-.85-1.346-1.894-1.346-1.1 0-1.832.53-1.832 1.34 0 .642.472 1.01 1.64 1.284l.987.243c1.838.43 2.596 1.178 2.596 2.53 0 1.72-1.33 2.799-3.453 2.799-1.987 0-3.323-1.029-3.422-2.637z" fill="black" fill-rule="nonzero"></path></svg></p>

```
// no easy solution
```

</div>
<div style="flex: 1; overflow: scroll">
<p style="margin-bottom:31px"><img src="/images/posts/remix.svg" alt="Remix"/></p>

```
// pages/todos/$id.tsx

import {
  ErrorBoundaryComponent,
  Links,
  Meta,
  Scripts,
} from "remix";

// ...(rest of component)

export function ErrorBoundary({ error }: ErrorBoundaryComponent) {
  console.error(error);
  return (
    <html>
      <head>
        <title>Oh no!</title>
        <Meta />
        <Links />
      </head>
      <body>
        <p>Nice error screen</p>
        <Scripts />
      </body>
    </html>
  );
}

```

</div>
</div>

### Increased Performance

Performance between these two frameworks is another hotly contested topic. Does SSG or SSR win? Looking at the below screenshots for our mickey mouse app, we see that Remix seems to outperform Next. Please take this with a grain of salt as this is not official at all.

Remix loaded in 378ms, while Next took 1.16s. In the links section at the end of this article, <a href="https://twitter.com/RyanFlorence">@RyanFlorence</a> has an article with a more in depth comparison.

<div style="display:flex;gap: 40px;margin-top: 20px">

<div style="flex: 1; overflow: scroll">
<p><svg height="40" style="transform:translateX(4%);shape-rendering:auto" version="1.1" viewBox="0 0 148 90" width="82" xmlns:xlink="http://www.w3.org/1999/xlink"><path d="M34.992 23.495h27.855v2.219H37.546v16.699h23.792v2.219H37.546v18.334h25.591v2.219H34.992v-41.69zm30.35 0h2.96l13.115 18.334 13.405-18.334L113.055.207 83.1 43.756l15.436 21.429H95.46L81.417 45.683 67.316 65.185h-3.018L79.85 43.756 65.343 23.495zm34.297 2.219v-2.219h31.742v2.219h-14.623v39.47h-2.554v-39.47H99.64zM.145 23.495h3.192l44.011 66.003L29.16 65.185 2.814 26.648l-.116 38.537H.145v-41.69zm130.98 38.801c-.523 0-.914-.405-.914-.928 0-.524.391-.929.913-.929.528 0 .913.405.913.929 0 .523-.385.928-.913.928zm2.508-2.443H135c.019.742.56 1.24 1.354 1.24.888 0 1.391-.535 1.391-1.539v-6.356h1.391v6.362c0 1.808-1.043 2.849-2.77 2.849-1.62 0-2.732-1.01-2.732-2.556zm7.322-.08h1.379c.118.853.95 1.395 2.149 1.395 1.117 0 1.937-.58 1.937-1.377 0-.685-.521-1.097-1.708-1.377l-1.155-.28c-1.62-.38-2.36-1.166-2.36-2.487 0-1.602 1.304-2.668 3.26-2.668 1.82 0 3.15 1.066 3.23 2.58h-1.354c-.13-.828-.85-1.346-1.894-1.346-1.1 0-1.832.53-1.832 1.34 0 .642.472 1.01 1.64 1.284l.987.243c1.838.43 2.596 1.178 2.596 2.53 0 1.72-1.33 2.799-3.453 2.799-1.987 0-3.323-1.029-3.422-2.637z" fill="black" fill-rule="nonzero"></path></svg></p>

<p style="margin-bottom:31px"><a href="/images/posts/next-performance.png"><img src="/images/posts/next-performance.png" alt="Next"/></a></p>

</div>
<div style="flex: 1; gap: 10; overflow: scroll">
<p style="margin-bottom:17px"><img src="/images/posts/remix.svg" alt="Remix"/></p>

<p style="margin-bottom:31px"><a href="/images/posts/remix-performance.png"><img src="/images/posts/remix-performance.png" alt="Remix"/></a></p>

</div>
</div>

### Bonus Points (add todo functionality)

To add some fun to our app I figured we could create add todo functionality that pulls from the <a href="https://www.boredapi.com/api/activity">bored api</a>.. No new concepts here but I'd suggest trying this out yourself and seeing which framework's mindset works better for you. If you get stuck feel free to look at the code below.

<div style="display:flex;gap: 40px;margin-top: 20px">

<div style="flex: 1; overflow: scroll">
<p><svg height="40" style="transform:translateX(4%);shape-rendering:auto" version="1.1" viewBox="0 0 148 90" width="82" xmlns:xlink="http://www.w3.org/1999/xlink"><path d="M34.992 23.495h27.855v2.219H37.546v16.699h23.792v2.219H37.546v18.334h25.591v2.219H34.992v-41.69zm30.35 0h2.96l13.115 18.334 13.405-18.334L113.055.207 83.1 43.756l15.436 21.429H95.46L81.417 45.683 67.316 65.185h-3.018L79.85 43.756 65.343 23.495zm34.297 2.219v-2.219h31.742v2.219h-14.623v39.47h-2.554v-39.47H99.64zM.145 23.495h3.192l44.011 66.003L29.16 65.185 2.814 26.648l-.116 38.537H.145v-41.69zm130.98 38.801c-.523 0-.914-.405-.914-.928 0-.524.391-.929.913-.929.528 0 .913.405.913.929 0 .523-.385.928-.913.928zm2.508-2.443H135c.019.742.56 1.24 1.354 1.24.888 0 1.391-.535 1.391-1.539v-6.356h1.391v6.362c0 1.808-1.043 2.849-2.77 2.849-1.62 0-2.732-1.01-2.732-2.556zm7.322-.08h1.379c.118.853.95 1.395 2.149 1.395 1.117 0 1.937-.58 1.937-1.377 0-.685-.521-1.097-1.708-1.377l-1.155-.28c-1.62-.38-2.36-1.166-2.36-2.487 0-1.602 1.304-2.668 3.26-2.668 1.82 0 3.15 1.066 3.23 2.58h-1.354c-.13-.828-.85-1.346-1.894-1.346-1.1 0-1.832.53-1.832 1.34 0 .642.472 1.01 1.64 1.284l.987.243c1.838.43 2.596 1.178 2.596 2.53 0 1.72-1.33 2.799-3.453 2.799-1.987 0-3.323-1.029-3.422-2.637z" fill="black" fill-rule="nonzero"></path></svg></p>

```
// pages/layouts/TodoLayout.tsx

import Link from "next/link";
import { ReactNode } from "react";
import { Todo } from "../pages/todos";
import { useRouter } from "next/router";

type TodoLayoutProps = {
  children: ReactNode;
  todos: Todo[];
};

export default function TodoLayout({ children, todos }: TodoLayoutProps) {
  const router = useRouter();
  const handleOnClick = async () => {
    const res = await fetch("http://localhost:3000/api/addTodo", {
      method: "post",
    });

    const json = await res.json();

    if (json.success) {
      router.push("/todos");
    }
  };
  return (
    <div>
      <h1>Todos</h1>
      <div className="header">
        <button onClick={handleOnClick}>Add Todo</button>
        <ul>
          {todos.map((todo) => (
            <li key={todo.id}>
              <Link href={`/todos/${todo.id}`}>{todo.title}</Link>
            </li>
          ))}
        </ul>
      </div>
      {children}
    </div>
  );
}
```

```
// pages/api/addTodo.ts

import { PrismaClient } from "@prisma/client";
import type { NextApiRequest, NextApiResponse } from "next";

export default async function handler(
  request: NextApiRequest,
  response: NextApiResponse
) {
  const prisma = new PrismaClient();

  const res = await fetch("https://www.boredapi.com/api/activity");
  const json = await res.json();

  await prisma.todo.create({
    data: {
      title: json.activity,
    },
  });

  return response.json({ success: true });
}
```

</div>
<div style="flex: 1; overflow: scroll">
<p style="margin-bottom:31px"><img src="/images/posts/remix.svg" alt="Remix"/></p>

```
// app/routes/todos.tsx

import { PrismaClient } from "@prisma/client";
import { Outlet } from "@remix-run/react";
import { Link, useLoaderData, Form } from "@remix-run/react";
import type { ActionFunction, LoaderFunction } from "remix";
import { redirect } from "remix";

export type Todo = {
  id: number;
  title: string;
};

export const loader: LoaderFunction = async () => {
  const prisma = new PrismaClient();
  const todos = await prisma.todo.findMany();
  return todos;
};

export const action: ActionFunction = async () => {
  const prisma = new PrismaClient();
  const res = await fetch("https://www.boredapi.com/api/activity");
  const json = await res.json();

  await prisma.todo.create({
    data: {
      title: json.activity,
    },
  });

  return redirect("/todos");
};

export default function Index() {
  const todos = useLoaderData<Todo[]>();
  return (
    <div>
      <h1>Todos</h1>
      <div className="header">
        <Form method="post">
          <button type="submit">Add Todo</button>
        </Form>
        <ul>
          {todos.map((todo) => (
            <li key={todo.id}>
              <Link to={`/todos/${todo.id}`}>{todo.title}</Link>
            </li>
          ))}
        </ul>
      </div>
      <Outlet />
    </div>
  );
}
```

</div>
</div>

## Conclusion

In conclusion, both of these frameworks are great and have their purpose. If you require SSG then your best bet is Next. Next is also a bit more popular at this time so you can find more resources on it. It allows a hybrid approach which is still one of a kind. Opting into SSG or SSR on a per page basis is great.

Remix on the other hand seems to be the right choice for SSR apps. Nested routing, actions, easier error boundaries, and typescript by default are the main advantages. You also don't lose anything on the data fetching side with loaders.

Another thing to notice is the lack of `useEffect` and `useState` especially in Remix. All the data fetching is done server side so it is available on component mount. I don't know about you but I enjoy this. Especially with React 18 on the horizon and the changes to `useEffect`. This <a href="https://www.youtube.com/watch?t=3994&v=Ck-e3hd3pKw&feature=youtu.be">talk</a> by <a href="https://twitter.com/DavidKPiano">@DavidKPiano</a> is great on this topic.

The other thing Remix does better is give you hooks for <a href="https://remix.run/docs/en/v1/guides/optimistic-ui">pending and optimistic ui</a> but I will let you explore that on your own.

## Links To Learn More

&bull; <a href="https://nextjs.org/docs/getting-started">Next Docs</a><br />
&bull; <a href="https://remix.run/docs/en/v1">Remix Docs</a><br />
&bull; <a href="https://remix.run/blog/remix-vs-next">Remix vs Next</a><br />
&bull; <a href="https://leerob.io/blog/image-gallery-supabase-tailwind-nextjs">Building an Image Gallery with Next.js, Supabase, and Tailwind CSS</a><br />
