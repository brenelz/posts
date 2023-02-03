---
title: "How To UseEffect"
date: "2023-02-03"
slug: "how-to-use-effect"
---

`useEffect` is a complicated React hook that I am still learning about. I
recently learned how to properly do things on mount while keeping the
`react-hooks/exhaustive-dep` lint rule happy.

## Confusing way

In the below code we setup a `useEffect` that gets triggered whenever `nextId`
changes. This effect will run on mount as well as when we click the button. I
think this is kinda confusing because you might not realize clicking the next
button will refetch the data (especially as the component grows in size).

```
function App() {
  const [nextId, setNextId] = useState(1);
  const [data, setData] = useState();

  async function getData(id) {
    const response = await fetch('https://jsonplaceholder.typicode.com/todos/' + id);
    const data = await response.json();

    setData(data);
  }

  useEffect(() => {
    getData(nextId);
  }, [nextId]);

  return (
    <div className="App">
        <button onClick={async () => {
            const nextNextId = nextId + 1;
            setNextId(nextNextId)
        }}>
            Next
        </button>
        <p>
            {data?.title}
        </p>
    </div>
  )
}
```

### Better way

The following is a slightly better way of structuring it (in my opinion). You
are more explicit as to what happens when the next button is clicked.

```
function App() {
  const [nextId, setNextId] = useState(1);
  const [data, setData] = useState();

  async function getData(id) {
    const response = await fetch('https://jsonplaceholder.typicode.com/todos/' + id);
    const data = await response.json();

    setData(data);
  }

  useEffect(() => {
    getData(nextId);
  }, []); // React Hook useEffect has a missing dependency: 'nextId'.

  return (
    <div className="App">
        <button onClick={async () => {
            const nextNextId = nextId + 1;
            getData(nextNextId);
            setNextId(nextNextId)
        }}>
            Next
        </button>
        <p>
            {data?.title}
        </p>
    </div>
  )
}
```

Now if we look at the `useEffect` closer we run into a problem. I only want this
to run on mount so it makes sense to specify an empty dependency array. To our
dismay, the hooks lint rule is yelling at us to supply a dependency.

```
useEffect(() => {
    getData(nextId);
}, []); // React Hook useEffect has a missing dependency: 'nextId'.
```

One non-recommended way to solve this is to eslint disable.

```
useEffect(() => {
    getData(nextId);
    // eslint-disable-next-line react-hooks/exhaustive-deps
}, []);
```

As an alternative we can add the dependency to make the linter happy.

```
useEffect(() => {
    getData(nextId);
}, [nextId]);
```

We have just introduced a bug, can you guess what it is?

Now when we click our button the `onClick` handler will run as well as the
effect which will cause our api call to be done twice. Thats not what we want!

A common way to solve this is use a ref to track the first mount.

```
useEffect(() => {
   if (!hasMountedRef.current) {
     hasMountedRef.current = true;
     getData(nextId);
   }
 }, [nextId]);
```

This just seems overly complex and we have another solution up our sleeve.

### Best way

The following is what I believe is the best way. Our lint rule is happy and we
don't make duplicate api calls!

```
const NEXT_ID_INITIAL = 1;

function App() {
  const [nextId, setNextId] = useState(NEXT_ID_INITIAL);
  const [data, setData] = useState();

  async function getData(id) {
    const response = await fetch('https://jsonplaceholder.typicode.com/todos/' + id);
    const data = await response.json();

    setData(data);
  }

  useEffect(() => {
    getData(NEXT_ID_INITIAL);
  }, []);

  return (
    <div className="App">
        <button onClick={async () => {
            const nextNextId = nextId + 1;
            getData(nextNextId);
            setNextId(nextNextId)
        }}>
            Next
        </button>
        <p>
            {data?.title}
        </p>
    </div>
  )
}
```

So let's break this down a bit. We create our `useEffect` with an empty
dependency array which essentially means run once on mount.

```
useEffect(() => {
    getData(NEXT_ID_INITIAL);
}, []);
```

We've also moved the initial value outside our component to a variable and
provided it to the initial `useState`

```
const NEXT_ID_INITIAL = 1;

function App() {
  const [nextId, setNextId] = useState(NEXT_ID_INITIAL);
```

I think this actually makes a lot of sense. When we call `getData` on initial
mount we are essentially using the initial state. By using that instead of the
`nextId` from `useState` we are more clear this is a static state and not using
a variable that could change.

## Other interesting resources

&bull; <a href="https://beta.reactjs.org/learn/you-might-not-need-an-effect">You
Might Not Need an Effect</a>
