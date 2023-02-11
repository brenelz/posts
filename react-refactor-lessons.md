---
title: "React Refactor Lessons"
date: "2023-02-11"
slug: "react-refactor-lessons"
---

So lately I've been on a bit of a React kick. I've felt pretty confident in
building things in React for awhile now but lately have been thinking about how
to build better components.

Usually when I create toy projects for fun I just shove everything in one big
file because it just makes it easier.

For an example of this see
<a href="https://github.com/brenelz/tennis-trivia/blob/finish/pages/index.tsx">this
file</a>.

I recently took the challenge of trying to extract some components on my Tennis
Trivia app and it was more challenging than I thought. Of course I could just
move things around and pass a ton of props everywhere but I wanted a better way.
I wanted to really think about how I was doing things.

I ended up with
<a href="https://github.com/brenelz/tennis-trivia/commit/223dd3acfbb11e6251dc9b57d5737d39df580af4">this
solution</a> after lots of back and forth.

I'm gonna be honest though that I had to start and stop the refactor project
multiple times as I just couldn't seem to come to a solution I was happy with.

I am fairly happy with it now and I want to share my thought process and things
I learned.

## Splitting into separate components

A core concept of React is components and this is where I wanted to start. It
allows you to keep one part of your app in your head instead of the whole thing.

Now for my little toy app its probably not required to have separate
maintainable components, but in larger scale real apps it is.

So the question then becomes what becomes a component. This wasn't really clear
to me as there is lots of options but I essentially settled on a `GameStep`
component.

Once I had that I also extracted a `GameForm` and a `GuessResult` component.

So what are good candidates for components? I'd say things with branches in
code, and forms.

This leads into my next thought which is where does your state belong?

## Where does your state belong

So this was the next question I asked myself when thinking about splitting out
components. What state is truly more global to my game, and what are just
temporary states.

Initially I had 5 `useState` calls in my `Home` component. Anytime you have a
bunch of these I usually think of it as a code smell. Now how many is too many?
I think it depends on your app size.

```
const [currentStep, setCurrentStep] = useState(0);
const [score, setScore] = useState(0);
const [pickedCountry, setPickedCountry] = useState("");
const [playersData, setPlayersData] = useState(players);
const [status, setStatus] = useState(null);
```

So back to the question. Which of these are temporary states? The two I
identified is `pickedCountry` and `status`. I feel that `currentStep`, `score`,
and `playersData` are more global and belong in the outermost component.

`pickedCountry` and `status` are more specific to the particular step I'm on
than to the game as a whole.

So what does this look like? I came up with something like the following:

```
export default function GameStep({ playerCountry, nextStep, increaseScore }: GameStepProps) {
    const [result, setResult] = useState<Result>(null);
}

export default function GameForm({ guessCountry }: GameFormProps) {
    const [pickedCountry, setPickedCountry] = useState("");
}
```

## Avoid having too many props

So another metric I use is how many props are being passed. As you can see in
the above they don't have a long list of props.

This is helped by the fact that the states now belong in the right place.
Initially i was passing setters all over the place and that ballooned the amount
of props each component was getting.

I suggest really thinking about which props are needed by what component and
which props you are just prop drilling and passing along.

My next tip is what many say is the hardest thing in software. Naming things!

## Naming is important

So this takes many shapes but in relation to the above I would suggest not just
passing down things like `setScore` to the child component. Instead pass down a
readable function that describes an action.

In my example I refactored it down to an `increaseScore` function that increases
the score by 1 and a `nextStep` function which increases `currentStep` by 1.

Also I initially had something called `status` but refactored it to
`guessResult` which I feel is much more clear as to what it actually is.

After all that I have mentioned above, I have saved my biggest aha moment for
last.

## How to clear state effectively

When looking at my original code I found I was doing a lot of complex logic
surrounding clearing of state. Things like this:

```
const nextStep = () => {
    setPickedCountry("");
    setStatus(null);
    setCurrentStep(currentStep + 1);
};

const playAgain = async () => {
    setPickedCountry("");
    setCurrentStep(0);
};
```

So after saying that it actually doesn't look that complicated but it was
interfering with me moving my state where I wanted to. How can `GameStep` and
`GameResult` own `status` and `pickedCountry` respectively but still have the
parent have the ability to clear it out.

My initial thought was that I could create a `useEffect` that clears out the
`status` and `pickedCountry` every time the `currentStep` changes.

```
export default function GameStep({ currentStep, playerCountry, nextStep, increaseScore }: GameStepProps) {
    const [result, setResult] = useState<Result>(null);
    const [pickedCountry, setPickedCountry] = useState('');

    useEffect(() => {
        setResult(null);
        setPickedCountry('');
    }, [currentStep])
};
```

`useEffect` is a powerful tool but I feel like it is reached for far too often.
In doing my refactor I was able to really think deeply about how I was using
them. In this case there is a better way to do this using the `key` in prop!

## How to use the key prop

So you'll notice that essentially we are just resetting the state to its initial
state every time the `currentStep` changes.

You can actually completely get rid of this `useEffect` and the `currentStep`
prop by using the `key` in the parent as follows:

```
<GameStep
    key={currentStep}
    increaseScore={increaseScore}
    playerCountry={player.country}
    nextStep={nextStep}
/>
```

Now React will essentially remount the component every time the `key` prop
changes thus achieving our goal. Simply understanding React's rendering and
mounting mechanisms avoids a lot of complicated code.

## Conclusion

So I know this was a lot to take in, and not sure how well I explained things.
It is fresh in my head and I have a lot of context of my specific Tennis Trivia
app.

I hoped you learned something!
