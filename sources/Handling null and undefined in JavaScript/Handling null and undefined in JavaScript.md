# Source

[[Medium] [Eric Elliott] Handling null and undefined in JavaScript](https://medium.com/javascript-scene/handling-null-and-undefined-in-javascript-1500c65d51ae)

# Sources of null & undefined

- User input
- Database/network records
- Uninitialized state
- Functions which could return nothing

# User Input

When you’re dealing with user input, validation is the first and best line  of defense. I often rely on schema validators to help with that job. For example, check out [react-jsonschema-form](https://rjsf-team.github.io/react-jsonschema-form/).

# Hydrating Records from Input

I always pass inputs I receive from the network, database, or user input through a hydrating function. For example, I’ll use [redux action creators](https://medium.com/javascript-scene/10-tips-for-better-redux-architecture-69250425af44) that can handle `undefined` values to hydrate user records:

```javascript
const setUser = ({ name = 'Anonymous', avatar = 'anon.png' } = {}) => ({
  type: setUser.type,
  payload: {
    name,
    avatar
  }
});
setUser.type = 'userReducer/setUser';
```

Sometimes, you’ll need to display different things depending on the  current state of the data. If it’s possible to display a page before all of the data is initialized, you may find yourself in that situation.  For example, when you’re displaying money balances to a user, you could  accidentally display a $0 balance before the data loads. I’ve seen this  upset users a number of times. You can create custom data types which  generate different outputs based on the current state:

```javascript
const createBalance = ({
  // default state
  state = 'uninitialized',
  value = createBalance.empty
} = {}) => createBalance.isValidState(state) && ({
  __proto__: {
    uninitialized: () => '--',
    initialized: () => value,
    format () {
      return this[this.getState()](value);
    },
    getState: () => state,
    set: value => {
      const test = Number(value);
      assert(!Number.isNaN(test), `setBalance Invalid value: ${ value }`);
      return createBalance({
        state: 'initialized',
        value
      });
    }
  }
});
createBalance.empty = '0';
createBalance.isValidState = state => {
  if (!['uninitialized', 'initialized'].includes(state)) {
    throw new Error(`createBalance Invalid state: ${ state }`);
  }
  return true;
};const setBalance = value => createBalance().set(value);const emptyBalanceForDisplay = createBalance()
  .format();
console.log(emptyBalanceForDisplay); // '--'const balanceForDisplay = setBalance('25')
  .format(balance);
console.log(balanceForDisplay); // '25'// Uncomment these calls to see the error cases:
// setBalance('foo'); // Error: setBalance Invalid value: foo// Error: createBalance Invalid state: THIS IS NOT VALID
// createBalance({ state: 'THIS IS NOT VALID', value: '0' });
```

The code above is a state machine which makes it impossible to display  invalid states. When you first create the balance, it will be set to an `uninitialized` state. If you try to display a balance while the state is `uninitialized`, you’ll always get a placeholder value ("`--`") instead.

To change that, you have to explicitly set a value by calling the `.set` method, or the `setBalance` shortcut we defined below the `createBalance` factory.

The state itself is [encapsulated](https://medium.com/javascript-scene/encapsulation-in-javascript-26be60e325b4) to protect it from outside interference to make sure that other functions can’t grab it and set it to an invalid state.

> *Note: If you’re wondering why we’re using strings instead of numbers for  this, it’s because I represent money types with big number strings with  lots of decimal precision to avoid rounding errors and accurately  represent values for cryptocurrency transactions, which can have  arbitrary significant decimal precision.*

If you’re using Redux or Redux architecture, you can declare state machines with [Redux-DSM](https://github.com/ericelliott/redux-dsm).

# Avoid creating `null` and `undefined` values

In your own functions, you can avoid creating `null` or `undefined` values to begin with. There are a couple ways to do that built into JavaScript that spring to mind. See below.

# Avoid null

I never explicitly create `null` values in JavaScript, because I never really saw the point of having  two different primitive values that essentially mean "this value does  not exist."

Since 2015, JavaScript has supported default values that get filled in when  you don’t supply a value for the argument or property in question. Those defaults don’t work for `null` values. That is usually a bug, in my experience. To avoid that trap, don’t use `null` in JavaScript.

If you want special cases for uninitialized or empty values, state machines are a better bet. See above.

# New JavaScript Features

There are a couple of features that can help you deal with `null` or `undefined` values. Both are stage 3 proposals at the time of this writing, but if  you're reading from the future, you may be able to use them.

As of this writing, optional chaining is a stage 3 proposal. It works like this:

```javascript
const foo = {};
// console.log(foo.bar.baz); // throws error
console.log(foo.bar?.baz) // undefined
```

## Nullish Coalescing Operator

Also a stage 3 proposal to be added to the specification, “nullish  coalescing operator” is basically a fancy way of saying “fallback value  operator”. If the value on the left is `undefined` or `null`, it evaluates to the value on the right. It works like this:

```javascript
let baz;
console.log(baz); // undefined
console.log(baz ?? 'default baz');
// default baz// Combine with optional chaining:
console.log(foo.bar?.baz ?? 'default baz');
// default baz
```

If the future hasn’t arrived, yet, you’ll need to install `@babel/plugin-proposal-optional-chaining` and `@babel/plugin-proposal-nullish-coalescing-operator`.

# Asynchronous Either with Promises

If a function may not return with a value, it might be a good idea to wrap it in an Either. In functional programming, the Either monad is a  special abstract data type that allows you to attach two different code  paths: a success path, or a fail path. JavaScript has a built-in  asynchronous Either [monad-ish data type](https://medium.com/javascript-scene/javascript-monads-made-simple-7856be57bfe8) called Promise. You can use it to do declarative error branching for undefined values:

```javascript
const exists = x => x != null;const ifExists = value => exists(value) ?
  Promise.resolve(value) :
  Promise.reject(`Invalid value: ${ value }`);
ifExists(null).then(log).catch(log); // Invalid value: null
ifExists('hello').then(log).catch(log); // hello
```

You could write a synchronous version of that if you want, but I haven’t  needed it much. I’ll leave that as an exercise for you. If you have a  good grounding in [functors](https://medium.com/javascript-scene/functors-categories-61e031bac53f) and [monads](https://medium.com/javascript-scene/javascript-monads-made-simple-7856be57bfe8), the process will be easier. If that sounds intimidating, don’t worry  about it. Just use promises. They’re built-in and they work fine most of the time.

# Arrays for Maybes

Arrays implement a `map` method which takes a function that is applied to each element of the  array. If the array is empty, the function will never be called. In  other words, Arrays in JavaScript can fill the role of Maybes from  languages like Haskell.

## What is a Maybe?

A Maybe is a special abstract data type that encapsulates an optional value. The data type takes two forms:

- Just — A Maybe that contains a value
- Nothing — a Maybe with no value

Here’s the gist of the idea:

```javascript
const log = x => console.log(x);
const exists = x => x != null;const Just = value => ({
  map: f => Just(f(value)),
});
const Nothing = () => ({
  map: () => Nothing(),
});
const Maybe = value => exists(value) ?
  Just(value) :
  Nothing();const empty = undefined;
Maybe(empty).map(log); // does not log
Maybe('Maybe Foo').map(log); // logs "Maybe Foo"
```

This is just an example to demonstrate the concept. You could build a whole  library of useful functions around maybes, implementing other operations like `flatMap` and `flat` (e.g., to avoid `Just(Just(value))` when you compose multiple Maybe-returning functions). But JavaScript  already has a data type that implements a lot of those features  out-of-the-box, so I usually reach for that instead: The Array.

If you want to create a function which may or may not produce a result  (particularly if there can be more than one result), you may have a  great use-case to return an array.

```javascript
const log = x => console.log(x);
const exists = x => x != null;const arr = [1,2,3];
const find = (p, list) => [list.find(p)].filter(exists);
find(x => x > 3, arr).map(log); // does not log anything
find(x => x < 3, arr).map(log); // logs 1
```

I find the fact that `map` won't be called on an empty list very useful for avoiding `null` and `undefined` values, but remember, if the array contains `null` and `undefined` values, it will call the function with those values, so if the function you're running could produce `null` or `undefined`, you'll need to filter those out of your returned array, as demonstrated above. That could have the effect of changing the length of the  collection.

In Haskell, there’s a function `maybe` that (like `map`) applies a function to a value. But the value is optional and encapsulated in a `Maybe`. We can use JavaScript's `Array` data type to do essentially the same thing:

```javascript
// maybe = b => (a => b) => [a] => b
const maybe = (fallback, f = () => {}) => arr =>
  arr.map(f)[0] || fallback;// turn a value (or null/undefined) into a maybeArray
const toMaybeArray = value => [value].filter(exists);// maybe multiply the contents of an array by 2,
// default to 0 if the array is empty
const maybeDouble = maybe(0, x => x * 2);const emptyArray = toMaybeArray(null);
const maybe2 = toMaybeArray(2);// logs: "maybeDouble with fallback:  0"
console.log('maybeDouble with fallback: ', maybeDouble(emptyArray));
// logs: "maybeDouble(maybe2):  4"
console.log('maybeDouble(maybe2): ', maybeDouble(maybe2));
```

`maybe` takes a fallback value, then a function to map over the maybe array,  then a maybe array (an array containing one value, or nothing), and  returns either the result of applying the function to the array's  contents, or the fallback value if the array is empty.

For convenience, I’ve also defined a `toMaybeArray` function, and curried the `maybe` function to make it most obvious for this demonstration.

If you’d like to do something like this in production code, I’ve created a unit tested open source library to make it easier. It’s called [Maybearray](https://github.com/ericelliott/maybearray). The advantage of Maybearray over other JavaScript Maybe libraries is  that it uses native JavaScript arrays to represent values, so you don’t  have to give them any special treatment or do anything special to  convert back and forth. When you encounter Maybe arrays in your  debugging, you don’t have to ask, “what is this weird type?!” It’s just  an array of a value or an empty array and you’ve seen them a million  times before.