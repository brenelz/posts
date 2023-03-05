---
title: "Solid vs React"
date: "2023-03-04"
slug: "solid-vs-react"
---

So lately there has been a lot of talk of signals on twitter. I don't have much
experience with them so I decided to try rewriting my React
<a href="https://github.com/brenelz/color-guesser">Color Guesser</a> app in
Solid and see what all the syntax differences are.

First let me explain a bit about Solid. Solid is founded on fine-grained
reactivity. It doesn't have a virtual dom and does dom updates in place as
things change making it more performant than alternatives. Another thing to note
is it uses jsx so that part should be familiar if you're coming from React.

I am going to focus primarily on syntax in this article but if you want
additional reading check these posts by
<a href="https://twitter.com/RyanCarniato">Ryan Carniato</a> out.

<a href="https://dev.to/this-is-learning/the-evolution-of-signals-in-javascript-8ob">The
Evolution of Signals in JavaScript</a><br />
<a href="https://dev.to/this-is-learning/react-vs-signals-10-years-later-3k71">React
vs Signals: 10 Years Later</a><br />
<a href="https://dev.to/this-is-learning/making-the-case-for-signals-in-javascript-4c7i">Making
the Case for Signals in JavaScript</a><br />

## Side-by-side comparison

### React

```
import React, { useState } from 'react';
import { generateColors, getCorrectColor } from './colors';
import Game from './Game';

const NUM_COLORS = 3;

function App() {
  const [game, setGame] = useState(0);

  const colors = generateColors(NUM_COLORS);
  const correctColor = getCorrectColor(colors);

  const nextGame = () => {
    setGame(game + 1);
  }

  return (
    <Game
      key={game}
      nextGame={nextGame}
      colors={colors}
      correctColor={correctColor}
    />
  )
}

export default App
```

### Solid

```
import Game from "./Game"

function App() {
  return (
    <Game />
  )
}

export default App
```

So your initial thought might be that Solid requires less code, but this isn't
actually the case. I was able to move the state within the `Game` component in
Solid.

In React I wanted the state to be in the `App` component so I could reset it
using the `key` prop when going to the next game.

An interesting mind shift had to occur in that I can't rely on a rerender as
Solid components only get ran once.

Let's look at some more code:

### React

```
import React, { useState } from 'react';

type GameProps = {
    nextGame: () => void;
    colors: string[];
    correctColor: string;
}

function Game({ nextGame, colors, correctColor }: GameProps) {
    const [guess, setGuess] = useState<string | undefined>(undefined);

    const wonGame = guess === correctColor;

    return (
        <div
            style={{
                display: "flex",
                flexDirection: "column",
                alignItems: "center",
                margin: 'auto',
            }}
        >
            <h1>Color Codes</h1>
            <h2>{correctColor}</h2>
            <h2>What color is this?</h2>

            <div data-testid="color-container" style={{ display: 'flex', gap: 10 }}>
                {colors.map(color => (
                    <button
                        key={color}
                        onClick={() => {
                            setGuess(color);
                        }}
                        style={{
                            height: 100,
                            width: 100,
                            cursor: 'pointer',
                            background: color
                        }}
                        data-testid={color === correctColor ? "correct-color" : "incorrect-color"}>
                    </button>
                ))}
            </div>

            {guess ?
                wonGame ? <div style={{ textAlign: 'center' }}>
                    <p>Correct!</p>
                    <p><button onClick={nextGame}>Play Again</button></p>
                </div>
                    : <p>Incorrect!</p>
                : null
            }
        </div>
    )
}

export default Game
```

### Solid

```
import { createSignal, For, Show } from "solid-js";
import { generateColors, getCorrectColor } from "./colors";

const NUM_COLORS = 3;

function Game() {
    const [colors, setColors] = createSignal(generateColors(NUM_COLORS));
    const [correctColor, setCorrectColor] = createSignal(getCorrectColor(colors()));
    const [guess, setGuess] = createSignal<string | undefined>(undefined);

    const nextGame = () => {
        setColors(generateColors(NUM_COLORS));
        setCorrectColor(getCorrectColor(colors()));
        setGuess('');
    }

    const wonGame = () => guess() === correctColor();

    return (
        <div
            style={{
                display: "flex",
                'flex-direction': "column",
                'align-items': "center",
                margin: 'auto',
            }}
        >
            <h1>Color Codes</h1>
            <h2>{correctColor}</h2>
            <h2>What color is this?</h2>

            <div data-testid="color-container" style={{ display: 'flex', gap: "10px" }}>
                <For each={colors()}>{color =>
                    <button
                        onClick={() => {
                            setGuess(color);
                        }}
                        style={{
                            height: "100px",
                            width: "100px",
                            cursor: 'pointer',
                            background: color
                        }}
                        data-testid={color === correctColor() ? "correct-color" : "incorrect-color"}>
                    </button>
                }</For>
            </div>

            <Show when={guess()}>
                <Show when={wonGame()} fallback={<p>Incorrect!</p>}>
                    <div style={{ 'text-align': 'center' }}>
                        <p>Correct!</p>
                        <p><button onClick={nextGame}>Play Again</button></p>
                    </div>
                </Show>
            </Show>
        </div >
    )
}

export default Game;
```

As I mentioned earlier I had to use more of `createSignal` than `useState`. This
is so I can change the colors when a new game is created. I can no longer rely
on the `key` prop and rerenders to do this for me.

The other interesting thing to note is you have to use the values as function
calls. For example `colors()` instead of just `colors`. This allows Solid to
maintain its reactivity. You also need to wrap derived state in a function so it
can stay reactive based on the values.

```
const wonGame = () => guess() === correctColor();
```

The other slight difference is css properties are not camel cased like react.
For example `flex-direction` instead of `flexDirection`.

Also Solid has control flow components you use instead of `map` and ternaries.
You can see in my example how I use `<Show>` and `<For>`. I am not totally sure
how I feel about this. In some ways I think it makes things a bit more readable
when things are nested, but it also doesn't feel just like JavaScript in the way
that React does.

## Conclusion

This has been a brief tour into some of the things I learned rewriting this app.
As mentioned I wanted to get a feel of how signals work in practicality.
Honestly the ergonomics are familiar to `useState` but the one interesting thing
is you can use them outside components unlike hooks! 🤯

```
const [colors, setColors] = createSignal(generateColors(NUM_COLORS));
const [correctColor, setCorrectColor] = createSignal(getCorrectColor(colors()));

function Game() {
    const [guess, setGuess] = createSignal<string | undefined>(undefined);
}
```

I can see this being useful for global state as you can also just export it
which is much simpler than using React Context.
