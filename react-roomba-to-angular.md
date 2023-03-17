---
title: "React Roomba To Angular"
date: "2023-03-16"
slug: "react-roomba-to-angular"
---

The next framework I've taken to learning is Angular. It's cool having these
small apps I've built that I can implement in different frameworks for a
learning experience.

In this case I went about porting my
<a href="https://github.com/brenelz/roomba-react">roobma-react</a> project to
<a href="https://github.com/brenelz/angular-roomba">angular</a>.

Overall the code translated over fairly well with some obvious differences.

## Component syntax

The below is nothing too crazy. The main difference is the element syntax where
one is lowercase and with a dash. `<app-roomba>` vs `<Roomba />`.

Angular also has separate `.html` templates while React has the concept of JSX
where the markup is right in the JavaScript.

#### Angular

```
// app.component.html

<div class="wrapper">
  <app-roomba></app-roomba>
</div>
```

#### React

```
// App.jsx

import React from 'react';
import Roomba from './components/Roomba';

function App() {
    return (
        <div className="wrapper">
            <Roomba />
        </div>
    )
}

export default App;
```

With React you don't have to do any special importing while in Angular you need
to bind your elements in a module like the following:

```
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';

import { AppComponent } from './app.component';
import { RoombaComponent } from './roomba/roomba.component';
import { RoombaGridComponent } from './roomba-grid/roomba-grid.component';

@NgModule({
  declarations: [
    AppComponent,
    RoombaComponent,
    RoombaGridComponent
  ],
  imports: [
    BrowserModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

## Prop binding

The next difference you will see below is how props are used. Angular has
special syntax where `()` are used for events such as `click`, and `[]` for
binding attributes to properties on your class.

#### Angular

```
// roomba/roomba.component.html

<div>
    <h1>Roomba</h1>
    <p>
        <button (click)="togglePointer()">Change Direction</button>
    </p>
    <app-roomba-grid [coord]="coord" [pointer]="pointer"></app-roomba-grid>
</div>
```

#### React

```
import React from 'react';
import Grid from './Grid';

export default function Roomba() {
    // business logic here

    return (
        <div>
            <h1>Roomba</h1>
            <p>
                <button onClick={togglePointer}>Change Direction</button>
            </p>
            <Grid coord={coord} pointer={pointer} />
        </div>
    )
}
```

## Control Flow

So here is where React makes its case as just being JavaScript. In Angular you
don't use `map` and `ternaries` but instead `*ngIf` and `*ngFor` in your
templates.

#### Angular

```
// roomba-grid/roomba-grid.component.html

<div *ngFor="let row of grid; let rowIndex = index" class="row">
    <div *ngFor="let column of row; let columnIndex = index" class="cell">
        <span *ngIf="coord[0] === rowIndex && coord[1] == columnIndex">{{pointer}}</span>
    </div>
</div>
```

#### React

```
import React from 'react';
import { range } from "../utils";

function Grid({ coord, pointer }) {
    const grid = range(5).map(_ => range(5).map(_ => ''));
    const [x, y] = coord;

    return (
        grid.map((row, rowIndex) => (
            <div key={rowIndex} className="row">
                {row.map((_, columnIndex) => (
                    <div key={columnIndex} className="cell">
                        {x === rowIndex && y == columnIndex ? pointer : null}
                    </div>
                ))}
            </div>
        ))
    )
}

export default Grid;
```

## Functions vs. Classes

You'll notice quite a few differences here:

&bull; React uses functions where Angular has leaned into classes<br /> &bull;
Instead of `useState` we just have instance variables on the class that we
modify directly using `this.`<br /> &bull; We use `ngOnInit` and `ngOnDestroy`
lifecycle hooks vs a `useEffect` hook to synchronize the timer.

#### Angular

```
// roomba/roomba.component.ts

import { Component } from '@angular/core';

@Component({
  selector: 'app-roomba',
  templateUrl: './roomba.component.html',
  styleUrls: ['./roomba.component.scss']
})
export class RoombaComponent {
  coord: [number, number] = [0, 0];
  pointer = '👉';
  timer: number | undefined;

  ngOnInit() {
    this.timer = window.setInterval(() => this.move(), 1000);
  }

  ngOnDestroy() {
    clearTimeout(this.timer);
  }

  togglePointer() {
    if (this.pointer === '👉') {
      this.pointer = '👇';
    } else if (this.pointer === '👇') {
      this.pointer = '👈';
    } else if (this.pointer === '👈') {
      this.pointer = '👆';
    } else if (this.pointer === '👆') {
      this.pointer = '👉';
    }
  }

  move() {
    // move logic
  }
}
```

#### React

```
import React from 'react';
import { useEffect } from 'react';

export default function Roomba() {

     const move = React.useCallback(() => {
        // move logic here
    }, [coord, pointer, togglePointer]);

    useEffect(() => {
        const timer = setInterval(move, 1000);

        return () => {
            clearTimeout(timer);
        }
    }, [move])

    // returned jsx here
}
```

## Props

The one last thing to quickly note below is that you use the `Input()` decorator
to show that these props are going to be passed in from the parent.

#### Angular

```
// roomba-gird/roomba-grid.component.ts

import { Component, Input } from '@angular/core';

function range(size: number, startAt = 0) {
  return [...Array(size).keys()].map(i => i + startAt);
}

@Component({
  selector: 'app-roomba-grid',
  templateUrl: './roomba-grid.component.html',
  styleUrls: ['./roomba-grid.component.scss']
})
export class RoombaGridComponent {
  @Input() coord!: [number, number];
  @Input() pointer!: string;

  grid = range(5).map(_ => range(5).map(_ => ''));
}
```

## 

This was my journey into Angular. I hope you enjoyed it!
