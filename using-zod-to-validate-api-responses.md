---
title: "Using Zod To Validate Api Responses"
date: "2023-01-09"
slug: "using-zod-to-validate-api-responses"
---

<a href="https://github.com/colinhacks/zod">Zod</a> is the latest tech I have
been trying out. It comes highly recommended from people I admire in the
TypeScript community.

Zod is a typescript-first schema validator. It is a validator similar to
<a href="https://github.com/jquense/yup">Yup</a> but has a better developer
experience and integrates more seamlessly with TypeScript.

The most obvious use case for validation is for user input, but I want to look
at another use case of validating your api responses. When to do this I'm still
going back and forth on. The general consensus is if you don't own the api it
its super important but if you own the api it is a little less necessary.
Validating your api responses can produce better error messages instead of a
dreaded undefined error.

## Example Using Deno

### Install Deno

`curl -fsSL https://deno.land/x/install/install.sh | sh`

This is not a Deno article but it is an alternative to Node.js that you can
<a href="https://deno.com/deploy/docs">read more about</a>.

### Setting Up Zod

In a folder create a file called `person.ts` with the below code.

```
// person.ts

import { z } from "https://deno.land/x/zod@v3.20.2/mod.ts";

const PersonSchema = z.object({
  name: z.string(),
  height: z.coerce.number(),
  films: z.array(z.string().url()),
  eye_color: z.enum(["green", "blue"]),
});

const FilmSchema = z.object({
  title: z.string(),
});

type Film = z.infer<typeof FilmSchema>;
```

Zod has a really simple interface with which you can define types such as
`z.number()`, `z.object()`, `z.array()`, etc... You will notice it is similar in
ways to TypeScript and it actually allows you to extra a type using `z.infer`.
It actually provides a bit more functionality then TypeScript where you can
define `z.string().min(5)` which would enforce a minimum of 5 characters. It
also supports `url()`, `email`, `uuid` which are super handy.

Read more in the
<a href="https://github.com/colinhacks/zod#basic-usage">docs</a> if you desire.

### Using Zod

At the end of `person.ts` add the following api call and validate the response
against the `PersonSchema`.

```
// person.ts
...

const res = await fetch('https://swapi.dev/api/people/1');
const person = PersonSchema.parse(await res.json());

console.log(person);
```

There are basically two ways of validating. They are `parse` and `safeParse`.
The first throws an error if its invalid and the second doesn't error but
provides a `success: false` boolean.

### Reusable Validated Fetch

I tried my hand at creating some utilities for getting a typed and validated
response back from the normal fetch and this is what I came up with. It may look
a bit funky with all the TypeScript generic syntax but it preserves some good
types.

I recommend working through this
<a href="https://github.com/total-typescript/typescript-generics-workshop">Total
Typescript workshop</a> if you are unfamiliar with TypeScript generics.

```
// utils/fetch.ts

import { z } from "https://deno.land/x/zod@v3.20.2/mod.ts";

export const fetchTyped = async <T extends z.Schema>(url: string, schema: T, params?: RequestInit): Promise<z.infer<T>> => {
    const res = await fetch(url, {
        headers: {
            'content-type': 'application/json;charset=UTF-8',
        },
        ...params
    });

    return schema.parse(await res.json())
}

export const fetchTypedPost = <T extends z.Schema>(url: string, schema: T, body: Record<string, unknown>, params?: RequestInit) => {
    return fetchTyped(url, schema, {
        method: 'POST',
        body: JSON.stringify(body),
        ...params
    })
}
```

### Using New Utility Functions

```
// person.ts

const person = await fetchTyped('https://swapi.dev/api/people/1', PersonSchema);
//  ^? {name: string, height: number, films: string[], eye_color: 'green' | 'blue'}

const films = await Promise.all(person.films.map(filmUrl => fetchTyped(filmUrl, FilmSchema)));

const fullPerson = {
    person,
    films
};

console.log(fullPerson);
```

#### Promise.all

Here I found a good use case for `Promise.all`. We get a `string[]` back from
the api and need to make a `fetchTyped` call for each url to get the film
specific details.

### Another Example With Posts

The following is just another quick example doing `POST` instead of a `GET`
call.

```
// post.ts

import { z } from "https://deno.land/x/zod@v3.20.2/mod.ts"; import {
fetchTypedPost } from "./utils/fetch.ts";

const PostSchema = z.object({
    title: z.string(),
    body: z.string(),
    userId: z.number(),
    id: z.number()
});

const post = await fetchTypedPost('https://jsonplaceholder.typicode.com/posts',
PostSchema, { title: 'foo', body: 'bar', userId: 1, });

console.log(post);
```

## Conclusion

Will you be using Zod in the future. I'd love to hear your thoughts on if this
is overkill or not.

Full source code for these examples is located on
<a href="https://github.com/brenelz/deno-zod">GitHub</a>.

## Additonal Reading

&bull;
<a href="https://timdeschryver.dev/blog/why-we-should-verify-http-response-bodies-and-why-we-should-use-zod-for-this">Why
we should verify HTTP response bodies, and why we should use zod for this</a>
