> Many programming languages support programming in both functional and  imperative style but the syntax and facilities of a language are typically optimised for only one of these styles, and social factors like coding conventions and libraries often force the programmer towards one of the styles.
>
> https://wiki.haskell.org/Functional_programming

# "Maybe" data type

A Maybe is a special abstract data type that encapsulates an optional value. The data type takes two forms:

- Just — A Maybe that contains a value
- Nothing — a Maybe with no value

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



# Resources

- [Refactoring to Immutability - Kevlin Henney](../sources/Refactoring to Immutability - Kevlin Henney)
- [Handling null and undefined in JavaScript](../sources/Handling null and undefined in JavaScript)
- [Functional architecture - The pits of success - Mark Seemann](../sources/Functional architecture - The pits of success - Mark Seemann/Functional architecture - The pits of success - Mark Seemann.md) 
- [An overview of programming paradigms](../sources/An overview of programming paradigms/An overview of programming paradigms.md)

