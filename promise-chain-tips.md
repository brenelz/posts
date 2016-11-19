---
extends: _layouts.post
section: postContent
title: "Quick Tip: Promise Chains"
date: "November 19, 2016"
---

It's pretty common to see promise chains in your Javascript code.

There are a few things to keep in mind that will make your code more readable and more bulletproof.

## A Bad Example

```
getNHLTeams()
    .then(function(teams) {
        // long function body here

        getScores()
            .then(function() {
                //
            })
            .then(function() {
                //
            });
    });
```

## A Good Example

```
getNHLTeams()
    .then(sortTeams)
    .then(pickFirstTeam)
    .then(getScores) // another promise
    .then(calculateWinStreak)
    .catch(handleError);
```

**A couple best practises are:**

&bull; Use named functions instead of anonymous ones<br />
&bull; Avoid nesting multiple promises<br />
&bull; Add a catch in case something fails<br />

You can see in the above example that nesting is avoided completely. This logic also contains no business logic but instead focuses on coordinating the order of which things need to run.

