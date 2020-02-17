# Source

[[Article] [Kent C. Dodds] Eliminate an entire category of bugs with a few simple tools](https://kentcdodds.com/blog/eliminate-an-entire-category-of-bugs-with-a-few-simple-tools)

# Intro

*How you can use a few simple static code analysis tools to avoid common programming bugs.*

You've probably heard of ESLint, Prettier, and Flow/TypeScript. These are static code analysis tools that are wildly popular in the JavaScript ecosystem. I consider them all testing tools

# ESLint

ESLint is the pluggable linting utility for JavaScript. Linting is the process of analyzing code for potential errors without actually running the code. Consider this code:

```js
if (!'serviceWorker' in navigator) {
  // the user's using an old browser :-(
}
```

Do you spot the problem? If you do, that's great! But don't you think it'd be cool to not have to use your brain power to find and correct subtle bugs like this one? I do! Make a computer do as much of my work for me as possible, please and thank you. That's what ESLint does for you.

# Prettier

Prettier is the JavaScript code formatter. It'll take your code however you write it, and reformat it in a way that's consistent and legible every time. People often give me quizzical looks when I refer to Prettier as a testing tool. But check this out:

```js
const a = false
const b = false
const c = true
const d = a && b || c
```

What's the value of `d` here? Do you know the order of operations of those operators by heart? If you do, great! But do you trust that all the engineers on your team know them well enough to not introduce a bug when refactoring this?

Run that code through Prettier, and this is what you get:

```js
const a = false
const b = false
const c = true
const d = (a && b) || c
```

Even if you do know the order of operations, the extra parentheses — which Prettier automatically adds when you save the file — are quite helpful. And if you realize that's not what you wanted, then you can add the parentheses yourself and Prettier will leave it that way (`const d = a && (b || c)`).

This is one example of things Prettier does to make the intent of your code more obvious — freeing your brain to focus on harder problems.

### Flow/TypeScript

These are static type checkers for JavaScript. A static type checker adds syntax to JavaScript to allow you to specify what data type a variable is. It can follow that variable through the code to make sure it's being used properly. (No more `x is not a function`.)

Can you spot the bug in this code?

```js
function getFullName(user) {
  const {
    name: {first, middle, last},
  } = user
  return [first, middle, last].filter(Boolean).join('')
}

getFullName({first: 'Joe', middle: 'Bud', last: 'Matthews'})
```

Maybe you can, maybe you can't. Maybe your coworkers can, maybe they can't. In any case, wouldn't it be cool if we had some software that could spot the issue for us? If we run that code with Flow, here's what we get:

```shell
Error ┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈ flow-example.js:8:13

Cannot call getFullName with object literal bound to user because
property name is missing in object literal [1].

      5│   return [first, middle, last].filter(Boolean).join('')
      6│ }
      7│
 [1]  8│ getFullName({first: 'Joe', middle: 'Bud', last: 'Matthews'})
      9│
     10│
```

So without changing our code at all, we get notified something's wrong. Nice! Now, what if we add type annotations?

```js
// @flow

type User = {
  name: {
    first: string,
    middle: string,
    last: string,
  },
}
function getFullName(user: User): string {
  const {
    name: {first, middle, last},
  } = user
  return [first, middle, last].filter(Boolean).join('')
}

getFullName({first: 'Joe', middle: 'Bud', last: 'Matthews'})
```

Now if we run Flow on this, the error is even more helpful:

```shell
Error ┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈ flow-example.js:15:13

Cannot call getFullName with object literal bound to user because
property name is missing in object literal [1] but exists in
User [2].

 [2] 10│ function getFullName(user: User): string {
     11│   const {name: {first, middle, last}} = user
     12│   return [first, middle, last].filter(Boolean).join('')
     13│ }
     14│
 [1] 15│ getFullName({first: 'Joe', middle: 'Bud', last: 'Matthews'})
     16│
     17|
```

I like to consider type definitions with Flow or TypeScript to be a form of inline automated tests. I strongly recommend you give it a shot if you haven't yet. Incremental adoption is possible with these tools (especially if you're already using babel, you can just start using `babel-preset-{flow,typescript}`). Try it out on your next feature and see what you think.

# References

-  [How to add testing to an existing project.md](../How to add testing to an existing project/How to add testing to an existing project.md) 
-  [Write tests. Not too many. Mostly integration.md](../Write tests. Not too many. Mostly integration/Write tests. Not too many. Mostly integration.md) 
-  [Common Testing Mistakes.md](../Common Testing Mistakes/Common Testing Mistakes.md)
-  [Static vs Unit vs Integration vs E2E Testing for Frontend Apps.md](../Static vs Unit vs Integration vs E2E Testing for Frontend Apps/Static vs Unit vs Integration vs E2E Testing for Frontend Apps.md) 