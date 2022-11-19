---
layout: ../../layouts/PostLayout.astro
title: "You can use map for that?"
date: "2016-12-12"
path: "/posts/you-can-use-map-for-that"
---

Lately it seems like I've been learning a lot about functions such as map, filter, reduce, and zip to transform data.

These core functions can be useful in many different circumstances (reactive programming, the functional style of React/Redux, Laravel collections, etc...)

Let's go through each of these functions super quick.

### Map

Map is used when you want to transform each value in an array into some entirely different value. This takes the most simplest form of:

`var addOne = (x) => x + 1 // add one to every element in array`

`[1, 2, 3].map(addOne) // [2, 3, 4]`

### Filter

Filter is used to choose which elements of an existing array should be present in the new array by doing a simple conditional check.

`var onlyEven = (x) => x % 2 === 0 // only keep even numbers`

`[1, 2, 3].map(onlyEven) // [2]`

### Reduce

Reduce is a bit more complicated, but basically it takes a group of elements, and combines them into one value. A good example of a reduce is a sum function. Reduce takes two parameters (a callback, and an initial value).

`var sum = (acc, current) => acc + current`

`[1, 2, 3].reduce(sum, 0) // 6`

### Zip

Zip does exactly what it says. If you imagine a zipper you know it combines teeth from both sides. It essentially allows you turn turn this:

`['foo', 'bar'], ['apples', 'grapes'] ]`

to this:

`[ ['foo', 'apples'], ['bar', 'grapes'] ]`

## Transforming Data

These function examples are super basic, and because of this might not seem like they could be useful in your day-to-day programming. To get around this fact I want to work through a fairly complex example just to show you a bunch of techniques that I have learned. We are dealing with data a lot in our programs, so being able to bend it to our will is super handy.

Lets take a look at a few examples of transforming a data source a couple different ways (Javascript, PHP, and Laravel Collections)

**These are the requirements:**

&bull; Do not mutate anything or use any loops<br/>
&bull; Nest each albums info directly under each photo<br/>
&bull; Only grab a videos title, thumbnail url, and the shortest caption<br/>

Turn the following:

```
var data = {
    albums: [
        {
            id: 1,
            title: "Animals"
        },
        {
            id: 2,
            title: "People"
        }
    ],
    photos: [
        {
            albumId: 1,
            id: 1,
            title: "accusamus beatae ad facilis cum similique qui sunt",
            url: "http://placehold.it/600/92c952",
            thumbnailUrl: "http://placehold.it/150/30ac17",
            captions: ['this is going be a long caption', 'shorter caption']
        },
        {
            albumId: 1,
            id: 2,
            title: "reprehenderit est deserunt velit ipsam",
            url: "http://placehold.it/600/771796",
            thumbnailUrl: "http://placehold.it/150/dff9f6",
            captions: ['adfkdsfds', 'sdf dfd afd dsfds fd', 'ab']
        },
        {
            albumId: 2,
            id: 3,
            title: "Donec vel consequat nibh. ",
            url: "http://placehold.it/600/442586",
            thumbnailUrl: "http://placehold.it/150/23447d",
            captions: ['a', 'ab', 'abc']
        }
    ]
}
```

Into something like this:

```
var results = {
    [
        {
            album: {
                id: 1,
                title: "Animals"
            }
            title: "accusamus beatae ad facilis cum similique qui sunt",
            thumbnailUrl: "http://placehold.it/150/30ac17",
            shortestCaption: 'shorter caption'
        }
    ]
}
```

So the mindset to accomplish this is to get all values that you need to create the output in scope at the same time, and then figure out how many levels deep you are in order to flatten down to a single dimension array at the end.

These flatten operations don't come in JS automatically so we will have to create a helper to do this. Our helper method looks something like the following:

```
// [[1], [2], [3]].concatAll() == [1, 2, 3]

Array.prototype.concatAll = function() {
  var results = [];
  this.forEach(function(subArray) {
    results.push.apply(results, subArray);
  });

  return results;
};
```

Take a crack at solving this yourself, and once you are done take a look at the provided solution below.

<hr />

## JS Solution:

```
var results = data.albums.map((album) => {
    return data.photos.
        filter((photo) => {
            return photo.albumId === album.id
        }).
        map((photo) => {

          var smallestCaption = photo.captions.reduce((smallestCaption, caption) => {
            return smallestCaption.length < caption.length ? smallestCaption : caption;
          });

          return {
            album: album,
            title: photo.title,
            thumbnailUrl: photo.thumbnailUrl,
            shortestCaption: smallestCaption
          };
    })
}).concatAll();
```

## PHP Solution:

```
$data = [
    "albums" => [
        [
            "id" => 1,
            "title" => "Animals"
        ],
        [
            "id" => 2,
            "title" => "People"
        ]
    ],
    "photos" => [
        [
            "albumId" => 1,
            "id" => 1,
            "title" => "accusamus beatae ad facilis cum similique qui sunt",
            "url" => "http://placehold.it/600/92c952",
            "thumbnailUrl" => "http://placehold.it/150/30ac17",
            "captions" => ['this is going be a long caption', 'shorter caption']
        ],
        [
            "albumId" => 1,
            "id" => 2,
            "title" => "reprehenderit est deserunt velit ipsam",
            "url" => "http://placehold.it/600/771796",
            "thumbnailUrl" => "http://placehold.it/150/dff9f6",
            "captions" => ['adfkdsfds', 'sdf dfd afd dsfds fd', 'ab']
        ],
        [
            "albumId" => 2,
            "id" => 3,
            "title" => "Donec vel consequat nibh. ",
            "url" => "http://placehold.it/600/442586",
            "thumbnailUrl" => "http://placehold.it/150/23447d",
            "captions" => ['a', 'ab', 'abc']
        ]
    ]
];


$results = array_map(function ($album) use ($data) {
  $photos = array_filter($data['photos'], function($photo) use ($album) {
     return $photo['albumId'] === $album['id'];
  });
  return array_map(function ($photo) use ($album) {
    $smallestCaption = array_reduce($photo['captions'], function ($smallestCaption, $caption) {
      return strlen($smallestCaption) < strlen($caption) ? $smallestCaption : $caption;
    }, $photo['captions'][0]);

    return [
            "album" => $album,
            "title" => $photo['title'],
            "thumbnailUrl" => $photo['thumbnailUrl'],
            "shortestCaption" => $smallestCaption
    ];
  }, $photos);
}, $data['albums']);

var_dump(call_user_func_array('array_merge', $results));
```

## Laravel Collections Solution

```
$results = $data['albums']->flatMap(function ($album) use ($data) {
    return $data['photos']->filter(function ($photo) use ($album) {
        return $photo['albumId'] == $album['id'];
    })->map(function($photo) use ($album) {

        $smallestCaption = $photo['captions']->reduce(function($smallestCaption, $caption) {
             return strlen($smallestCaption) < strlen($caption) ? $smallestCaption : $caption;
        });

        return [
            "album" => $album,
            "title" => $photo['title'],
            "thumbnailUrl" => $photo['thumbnailUrl'],
            "shortestCaption" => $smallestCaption
        ];
    });
});
```

All these solutions could be handled with foreach loops, but there is something to be said for being able to use all the functions at your disposal. Doing things this way does take some time to get used to, but I think it helps your programming in the long run.

#### If you would like to read more on functional programming, you can check out the following resources:

https://github.com/getify/Functional-Light-JS<br />
http://reactivex.io/learnrx/<br />
http://rxmarbles.com/<br />
https://adamwathan.me/2015/01/01/refactoring-loops-and-conditionals/<br />
https://www.youtube.com/watch?v=1uRC3hmKQnM
