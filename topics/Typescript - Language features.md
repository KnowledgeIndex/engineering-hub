# Optional Chaining

Optional chaining is a form of syntax to short circuit evaluations in the event that something is null or undefined.

```typescript
// Old Way
if (deathRay) {
  deathRay.FireAt(target);
}

// New Way
deathRay?.FireAt(target);
```

# Nullish Coalescing

Nullish coalescing refers to the use of the `??` operator in evaluating things that could potentially be null or undefined.

```typescript
// Old Way
var calculator = someCalculator;
if (calculator !== null && calculator !== undefined) {
  calculator = new Calculator();
}

// New Way
var calculator = someCalculator ?? new Calculator();
```

# Assertion Functions

Essentially they are functions which, if called without error, have  asserted something to TypeScript’s internal type interpreting code. This in turn allows the compiler to catch more specific issues based on the  facts now proved to be true.

```typescript
function assertIsNumber(input: any): asserts input is number {
    if (typeof(input) !== 'number') {
        throw new Error('Oh snap!')
    }
}

function getStandardFixedNumberString(input: string | number): string {
    assertIsNumber(input); // Remove this line and we won't compile

    return input.toFixed(2).toString();
}
```

# The Declare Keyword

Declare lets us combine the dynamic typing system with inheritance to essentially re-declare inherited properties

```typescript
class Person {
    // Extra logic here
}

class AwesomePerson extends Person {
    public doAwesomeStuff(): void {
        
    }
}

class Theme {
    users: Person[] = [];
    // Extra logic here
}

class DarkTheme extends Theme {
    declare users: AwesomePerson[];

    public doStuff() {
        for (var user of this.users) {
            user.doAwesomeStuff();
        }
    }
    
    // Extra logic here
}
```

# Uncalled Function Checks

```typescript
class AwesomePerson extends Person {
    public onlyDoesBoringThings(): boolean {
        return false;
    }
    public doAwesomeStuff(): void {}
    public doBoringStuff(): void {}
}

function handlePersonTask(person: AwesomePerson) {
    if (person.onlyDoesBoringThings) { // oops - no () so we're just checking for non-null
        person.doBoringStuff();
    } else {
        person.doAwesomeStuff();
    }
}
```

Here we meant to invoke `person.onlyDoesBoringThings` at line 10, but forgot the `()`‘s and are instead evaluating the function against null / undefined. The function is defined, so that condition evaluates as `true` even though invoking it would have returned `fasle`.

TypeScript 3.7 catches this error out of the box:

```
This condition will always return true since the function is always defined. Did you mean to call it instead?
```

# Resources

-  [How TypeScript 3.7 Helps Quality](https://killalldefects.com/2019/11/06/how-typescript-3-7-helps-quality/)