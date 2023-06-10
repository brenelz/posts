---
title: "React Server Components With Next.js"
date: "2023-06-10"
slug: "react-server-components-with-next"
---

So as you probably know I recently ran a meetup where I talked about React
Server Components. I figured I would try to turn it into a blog post. This is
when I thought of a service I had seen called
<a href="https://contenda.co/">Contenda</a> that converts a video into a blog
post. The below is the result with some tweaks from myself.

Let's jump into the topic of React server components and Next.js. To provide
some background, website development began with the Multi-Page Application (MPA)
architecture, such as PHP and Rails. In this approach, the server generates HTML
and sends it to the browser as a static page without client-side interactivity.
This MPA architecture marked the early days of building websites.

![Contenda image](https://prod.kfi.contenda.io/90334a17-9225-4802-abf2-b48d737e63b8/key-frame/fpm-20/cropped/00-08-54_idx-009.jpg)

Following the MPA architecture, web development shifted towards Single-Page
Applications (SPAs). SPAs moved the rendering from the server to the client
side, but this approach often resulted in an empty div tag which negatively
impacted SEO. As an example, a root div would lack content, relying on
JavaScript for content injection.

To address this issue, developers transitioned to Server-Side Rendering (SSR).
With SSR, HTML is rendered on the server, eliminating the empty div tag issue.
The server sends the full page to the browser, which then hydrates the document
with JavaScript for event listeners and other interactive elements.

## Embracing the Future of Web Development with React Server Components

A new paradigm in web development is React server components, which allows for
continuous rendering of components on the server. Unlike SSR, which only helps
with the initial page load by generating and sending HTML to the client, React
server components can stream content over time, providing features like loading
skeletons. This approach represents a shift from the SPA environment, which
typically doesn't involve a server. React server components reintroduce the
server and offer a different approach to web development. So, why choose React
server components? Let's explore further.

![Contenda image](https://prod.kfi.contenda.io/90334a17-9225-4802-abf2-b48d737e63b8/key-frame/fpm-20/cropped/00-11-15_idx-013.jpg)

React server components offer a seamless blend of server-side and client-side
rendering, combining the smooth page transitions of SPAs with the server-first
architecture of MPAs. This approach extends React beyond a client-side only
framework, enabling server components to have direct access to data sources like
databases or file systems.

By using JavaScript on both the client and server, you eliminate the need for an
API in the middle. React server components also allow for shipping less
client-side JavaScript to the browser, resulting in faster initial page loads
and better user experiences. An example of this is a markdown parser. The server
can send the final result to the browser without transferring large amounts of
JavaScript.

React server components also enable better code sharing between server and
client sides, ensuring type safety across boundaries. This quick rundown covers
some of the key benefits of React server components.

![Contenda image](https://prod.kfi.contenda.io/90334a17-9225-4802-abf2-b48d737e63b8/key-frame/fpm-20/cropped/00-12-12_idx-014.jpg)

Next.js is currently the primary way to use React server components, although
other frameworks may adopt them in the future. To start a new Next.js app, you
can use the following command:

```javascript
npx create-next-app rsc-next-movies
```

This command scaffolds a new app with TypeScript, ESLint, and Tailwind CSS. The
folder structure mainly revolves around the `app` directory, where you generate
routes, loading states, error boundaries, and more. The rest of the structure
should look familiar, including `.env`, `.eslintrc`, and `package.json` files.

![Contenda image](https://prod.kfi.contenda.io/90334a17-9225-4802-abf2-b48d737e63b8/key-frame/fpm-20/cropped/00-15-36_idx-015.jpg)

## Exploring File-Based Routing and Folder Structure in Next.js

One notable feature of Next.js compared to Create React App is its file-based
routing. In a Next.js app, folders map to URL segments. For example, a `movies`
folder would be linked to the URL `localhost:3000/movies`. The `page.tsx` file
is a special file path used to render pages in the app. There are other files
like `error.tsx`, `loading.tsx`, and various layouts, but the `page.tsx` file is
the essential component for a basic Next.js app. Here's an example of a React
component for a movies page:

```javascript
import React from "react";

const MoviesPage = () => {
  return (
    <div>
      {/* Your movies page content */}
    </div>
  );
};

export default MoviesPage;
```

This TypeScript component represents a movies page in a Next.js app using
file-based routing.

Nested routing in Next.js can be achieved using layout files, such as
`layout.tsx`, which apply to all route segments within their scope. This allows
you to create hierarchical structures in your application.

The new folder structure in Next.js allows you to co-locate your components and
module CSS directly within their respective folders, such as the `movies` folder
in this case. This is different from previous Next.js versions, where you might
have used a separate `components` directory.

One downside to this approach is that you might end up with multiple files named
`page.tsx`. This can make searching for a specific file in your code editor more
challenging, as you'll need to search by the folder name rather than the file
name. Overall, though, this new folder structure offers better organization and
modularity.

![Contenda image](https://prod.kfi.contenda.io/90334a17-9225-4802-abf2-b48d737e63b8/key-frame/fpm-20/cropped/00-18-03_idx-018.jpg)

## Embracing Server-First Rendering: React Server Components in Next.js

Next.js supports React server components, which require a mental shift from the
typical client-first approach. By default, every component in Next.js is a
server component, and nothing is sent to the client. This design choice is made
because the entry point of your app always starts at the server. However, server
components can't use hooks or browser APIs, which are reserved for client
components. To create a client component, you need to use `use client` at the
top of the file.

There are a few security considerations when using React server components. Be
cautious when passing sensitive information between server and client
components. Server components render on the initial load and only redraw when a
form action sends a request back to the server.

Overall, React server components in Next.js offer a new way to build web
applications, focusing on server-first rendering while maintaining the benefits
of client components. This approach results in better performance and user
experience, although it requires a different mindset and adherence to certain
conventions.

![Contenda image](https://prod.kfi.contenda.io/90334a17-9225-4802-abf2-b48d737e63b8/key-frame/fpm-20/cropped/00-18-57_idx-019.jpg)

Fetching data is a crucial aspect when working with React server components.
With a client-based approach, you would typically use a `useEffect` hook or a
library like React Query. Consider the following example:

```javascript
import React, { useEffect, useState } from "react";
import { getTrending } from "../api";
import styles from "../styles/Home.module.css";

export default function Movies() {
  const [trendingMovies, setTrendingMovies] = useState([]);

  useEffect(() => {
    async function getData() {
      const data = await getTrending();
      setTrendingMovies(data);
    }

    getData();
  }, []);

  return (
    <main className={styles.main}>
      <pre>{JSON.stringify(trendingMovies, null, 2)}</pre>
      <h1>RSCNextMovies</h1>
    </main>
  );
}
```

In this example, we have an array of trending movies as state, and a `useEffect`
hook that runs on mount. The hook calls a function to fetch trending movies and
then updates the state. This approach, however, involves a considerable amount
of boilerplate code just to fetch and render movie data. The key point here is
the need for a more efficient way to fetch data, such as using React server
components.

![Contenda image](https://prod.kfi.contenda.io/90334a17-9225-4802-abf2-b48d737e63b8/key-frame/fpm-20/cropped/00-25-57_idx-020.jpg)

React server components greatly simplify data fetching, as shown in this
example:

```javascript
import { getTrending } from "../api";
export default async function Movies() {
  const data = await getTrending();
}
```

In this case, the component is an async function that awaits the data directly.
Since this is a server component by default, `getTrending` can query the
database or make a fetch call. The need for a `useEffect` hook is eliminated,
resulting in a more concise and straightforward code.

However, this approach doesn't handle loading or error states directly. In
Next.js, you can use a `error.tsx` file in the `movies` folder, which the
framework automatically wraps as an error boundary. Additionally, you can
utilize React Suspense for handling loading states.

Overall, React server components provide a more efficient and concise way to
fetch data, with the server handling the process before sending the results to
the client.

![Contenda image](https://prod.kfi.contenda.io/90334a17-9225-4802-abf2-b48d737e63b8/key-frame/fpm-20/cropped/00-26-57_idx-021.jpg)

## Utilizing React Server Components for Streamlined Data Mutations and Improved Efficiency

Before React server components, processing data often required setting up an API
endpoint, handling onClick events, and managing state variables for input
fields. Here's an example of how this would be done:

```javascript
export default function onClickSubmitToAPIEndpoint() {
  fetch("https://example.com/api/endpoint", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(stateVars),
  })
    .then((response) => {
      if (!response.ok) {
        throw new Error("Network error");
      }
      return response.json();
    })
    .then((data) => console.log(data))
    .catch((error) => console.error(error));
}
```

```javascript
export default function InteractiveElement() {
  const [title, setTitle] = useState("");
  const [review, setReview] = useState("");

  const onChangeTitle = (e) => setTitle(e.target.value);
  const onChangeReview = (e) => setReview(e.target.value);

  const onSubmit = () => onClickSubmitToAPIEndpoint();

  return (
    <form>
      <input type="text" name="name" value={title} onChange={onChangeTitle} />
      <input
        type="text"
        name="review"
        value={review}
        onChange={onChangeReview}
      />
      <button onClick={onSubmit}>Add Trending Review Title</button>
    </form>
  );
}
```

This code creates a form with input fields and a submit button that sends data
to an API endpoint. The `useState` hook is used to manage the state of each
input field. However, with React server components, you no longer need to set up
API endpoints and can directly call functions, reducing boilerplate code and
simplifying data processing.

![Contenda image](https://prod.kfi.contenda.io/90334a17-9225-4802-abf2-b48d737e63b8/key-frame/fpm-20/cropped/00-29-06_idx-022.jpg)

With React server components, you can create a server action by using the
special `use server` notation. For example:

```javascript
"use server";

export async function addTrending(formData) {
  const title = formData.get("title");
  console.log(title);
}
```

In this case, the `addTrending` async function receives `formData`, and you can
access its `title` field using the `get` method. Server actions, such as this
one, are still in their alpha stage and not recommended for production use.
However, the React team envisions data mutations to be achieved through server
actions, simplifying the process of exporting server functions.

![Contenda image](https://prod.kfi.contenda.io/90334a17-9225-4802-abf2-b48d737e63b8/key-frame/fpm-20/cropped/00-29-54_idx-027.jpg)

To use a server action, such as the `addTrending` function, simply pass it to
the form's `action`. This allows the function to run on the server, streamlining
the process of handling data mutations and reducing the need for a separate API
route. Here's an example of how to incorporate the `addTrending` function into a
form:

```javascript
import { addTrending } from "./actions";
import styles from "./movies.module.css";

export default function Movies() {
  return (
    <main className={styles.main}>
      <h1>RSC Next Movies</h1>
      <form action={addTrending}>
        <input type="text" name="title" />
        <button>Add Trending Movie Title</button>
      </form>
    </main>
  );
}
```

In this example, the server action is capable of saving data to a database or
performing other tasks typically handled by API routes. By passing the server
action directly to the form, you significantly simplify your code and improve
efficiency.

![Contenda image](https://prod.kfi.contenda.io/90334a17-9225-4802-abf2-b48d737e63b8/key-frame/fpm-20/cropped/00-30-42_idx-029.jpg)

Another way to use server actions is by calling them directly from a client
component. For example, you can have a button with an `onClick` event that
triggers the server action:

In this scenario, even though the `onClick` event comes from the client, all the
logic is executed on the server, similar to a remote procedure call (RPC). This
approach simplifies the code and allows for seamless interaction between client
and server components.

When using server actions in React components, it's essential to be cautious
with sensitive data. For instance, if you have a secret key in a server
component and it's used within a server action, the key might be serialized and
exposed when transferring data between the server and the client. This blurring
of client and server boundaries can lead to security concerns.

## Comparig React Server Components to Other Frameworks

Comparing React server components to other frameworks like Remix, we can see
some similarities and differences. Remix has the concept of loaders and actions,
which serve a similar purpose as server actions in Next.js.

In Remix, you would use an async function loader and the `useLoaderData` hook to
fetch data. While both approaches allow for handling data, React server
components offer more composability by allowing multiple action functions and
loaders further down the component tree. Remix, on the other hand, hoists data
fetching at the top page level, which can be beneficial in some cases.

![Contenda image](https://prod.kfi.contenda.io/90334a17-9225-4802-abf2-b48d737e63b8/key-frame/fpm-20/cropped/00-33-21_idx-037.jpg)

Remix has chosen not to adopt server components yet, as it's easy to create a
waterfall of data fetching.

Comparing React server components to other frameworks like Astro, which is more
tailored for content-first websites, we can notice some similarities and
differences.

Astro has a concept called "islands," which resemble React server components in
some ways. For instance, you can place an `await` statement directly in the
front matter of Astro components:

Astro supports multiple frameworks, including React, Vue, and Svelte. You can
embed React components in Astro and use a directive like `client:load` to
control interactivity. This directive is similar to the `use client` directive
in React server components.

```javascript
<main>
  <h1>RSC Next Movies</h1>
  <MyReactComponent client:load />
</main>;
```

Though Astro is gaining popularity, it doesn't have an equivalent server action
API like React server components, which highlights the unique features of each
framework and how they approach data handling and component interaction.

![Contenda image](https://prod.kfi.contenda.io/90334a17-9225-4802-abf2-b48d737e63b8/key-frame/fpm-20/cropped/00-36-54_idx-038.jpg)

React's development journey has come full circle - it started on the server,
moved to the client, and now is heading back to the server. This shift can be
attributed to factors such as improved performance and the reduced cost of
server resources.

However, there has been some controversy regarding the React documentation not
prominently featuring Vite, a build tool and development server that primarily
supports single-page applications (SPAs). The new React docs are heavily focused
on server-centric solutions like Next.js and Remix.

As React continues to evolve, improvements in data mutations and server
components are expected. However, the ecosystem will need to adapt to this new
pattern of using `use client`. Developers may need to wrap third-party packages
in `use client` if they don't support React server components yet, which has led
to some pushback from the community.

It will be interesting to see how the ecosystem evolves and whether more
frameworks adopt React server components, expanding the available options beyond
Next.js.

![Contenda image](https://prod.kfi.contenda.io/90334a17-9225-4802-abf2-b48d737e63b8/key-frame/fpm-20/cropped/00-38-42_idx-040.jpg)

## Resources

A recommended resource for understanding React's journey towards server-side
rendering is Dan Abramov's talk at RemixConf, titled "React from Another
Dimension." In this talk, he describes how React would have evolved had it
started with a server-side approach before moving towards the client-side.

Additionally, the Next.js router documentation is an excellent resource for
learning more about server-side rendering with React.

These resources provide valuable insights into React's evolution, its
server-side rendering capabilities, and the future of the framework.
