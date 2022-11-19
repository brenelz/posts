---
layout: ../../layouts/PostLayout.astro
title: "Using React Hooks"
date: "2020-08-12"
path: "/posts/using-react-hooks"
---

The concept of react hooks is a bit of a new topic, or maybe not so anymore but I still know plenty of people unfamiliar with them. Some of the old apps I've built were based on class based components which is no longer the best practise.

Follow along with me and I hope I can explain them in an easy way that isn't just repeating the <a href="https://reactjs.org/docs/hooks-intro.html">docs</a>.

## useState

useState is the simplest and most common hook you will use. Remember setState? you will now use useState. A simple example is a component something like this:

```
import React, { useState } from 'react'

const Characters = () => {
    const [characters, setCharacters] = useState(null)

    const getCharacters = () => {
        setCharacters([
            { name: 'Rick' },
            { name: 'Morty' },
            { name: 'Mr. Meeseeks' }
        ])
    }

    return (
        <>
            <button onClick={getCharacters}>Get Characters</button>
            {characters ? characters.map(character => (
                <h1>{character.name}</h1>
            )) : <p>No characters yet</p>}
        </>
    )
}

export default Characters;
```

Basically `useState` is a function you call with an initial value (in this case null). It then gives you back a tuple where the first item contains the accessor of the value, and the second item is a setter of the value.

In the above case the `Characters` component would render once with characters set to null, which would spit out "No Characters yet" on the screen. Then once hitting the button it would run `setCharacters` and because React knows this is a state change it re-renders again. This time it will output the characters names to the screen.

A couple other common examples of tracking and managing state would be things like:

```
const [error, setError]   = useState(false)
const [isOpen, setIsOpen] = useState(false)
```

## useEffect

Now in our example above where we manually set the characters you probably aren't actually doing this. You are fetching from an api. This is where useEffect comes in. An effect is anything that can affect other components and can’t be done during rendering.

Let's write a simple `useEffect` to fetch from the Rick and Morty api.

```
import React, { useState, useEffect } from 'react'

const Characters = () => {
    const [characters, setCharacters] = useState(null)

    useEffect(async () => {
        const response = await fetch(`https://rickandmortyapi.com/api/character/`);
        const apiCharacters = await response.json()

        setCharacters(apiCharacters.results)
    }, [])

    return (
        <>
            {characters ? characters.map(character => (
                <h1>{character.name}</h1>
            )) : <p>No characters yet</p>}
        </>
    )
}

export default Characters;
```

Some things to note here is we are including {`useEffect } from 'react` just like we did with `useState` if you didn't notice earlier. We then call useEffect with 2 parameters. The first is a function to be executed that contains the side effect code; the second is a dependencies array. If your side effect depends on a variable this is where you would include it. In this case an empty array means to render once on mounting of the component.

As you can see we are just doing a simple fetch of json, and then calling the `setCharacters` as we did before but this time instead of waiting for a button click we are doing it on mount. This causes a re-render and thus lists our characters.

Another type of side effect would be manually manipulating the dom.

## useRickAndMorty (custom hooks)

Now that we have a useEffect call we can extra this out into its own file or function. As you can imagine some of these useEffect functions can get pretty long and end up making your component more complex than it needs to be. Also by refactoring this out you could use this in multiple places.

```
import React, { useState, useEffect } from 'react'

function useRickandMorty() {
    const [characters, setCharacters] = useState(null)

    useEffect(async () => {
        const response = await fetch(`https://rickandmortyapi.com/api/character/`);
        const apiCharacters = await response.json()

        setCharacters(apiCharacters.results)
    }, [])

    return [characters, setCharacters]
}

const Characters = () => {
    const [characters, setCharacters] = useRickandMorty()

    return (
        <>
            {characters ? characters.map(character => (
                <h1>{character.name}</h1>
            )) : <p>No characters yet</p>}
        </>
    )
}

export default Characters;
```

As you can see this cleans up our characters component a lot. Now we also have a reusable way to get Rick and Morty characters.

## useReducer

You can imagine your components getting quite long with `useState` and `useEffect`. We can do something a little different using a reducer. If you come from using Redux you might be familiar with this word, but basically you are changing state by using something like `state + action = new state`

Another benefit is you can do logic in your reducer instead of spreading it throughout your app.

```
import React, { useReducer } from 'react'

const initialState = {characters: null, won: false}

function characterReducer(state, action) {
    switch (action.type) {
        case 'WIN_GAME':
            return {...state, won: true};
        case 'LOSE_GAME':
            return {...state, won: false};
        case 'GET_CHARACTERS':
            return {...state, characters: action.characters};
        default:
            return state
    }
}

const Characters = () => {
    const [state, dispatch] = useReducer(characterReducer, initialState)

    const characters = state.characters;

    return (
        <>
            <button onClick={() => {
                dispatch({type: 'WIN_GAME'})
            }}>Win Game</button>
            {state.won && <p>You won!</p>}
            {characters ? characters.map(character => (
                <h1>{character.name}</h1>
            )) : <p>No characters yet</p>}
        </>
    )
}

export default Characters;
```

This may look like more code and it is but it allows us some flexibility. You can see we dispatch an action `WIN_GAME` but don't actually define the logic for this in the render part of our component. We can leave this up to the reducer to figure out. This allows this action to be dispatched in other areas of our app and we can reuse the functionality.

The `{...state, won: true}` might look a little strange, but basically this is using the spread operator. This allows us to not mutate objects and instead create a new one each time. We are basically saying to keep the state the same except change the won attribute.

## Conclusion

I hope this article has cleared some things up for you, but if not their are some great resources below that probably explain things a lot better than I do.

## Resources

&bull; <a href="https://scrimba.com/course/greusablereact">Build Reusable React (scrimba course)</a><br />
&bull; <a href="https://reactjs.org/docs/hooks-overview.html">Hooks at a Glance</a><br/>
&bull; <a href="https://www.youtube.com/watch?v=GLuPJzl_Nv4">React Workout: Intro to useState and useEffect (youtube)</a></br >
