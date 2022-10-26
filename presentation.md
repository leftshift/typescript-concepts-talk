# Advanced Typescript


Note:

Didn't find a better title yet. We'll start with the basics.

Important to really understand them so the later examples make sense.

///

# typescript?
* superset of javascript
* compiled to javascript


Note:

* *syntactically* correct js is syntactically correct ts

////

## statically typed

```ts twoslash
let a = 1;
//  ^?
```

Note:

* every variable has some known type at compile time which generally can't change
* no redeclaring with a different type

////

## strongly typed

```ts twoslash
// @errors: 2367
let a = '1';

a === 2;
```

Note:

Although I'd put a question mark behind that- it still has a lot of the weird type cohersion rules from javascript


////

## powerful type inference

```ts twoslash
const dishes = ["pasta", "pizza", "lasagne"];

const favourite = dishes[2];
//    ^?
```

///

# Types

////

## Convention

```ts twoslash

type pretty = 'everything ~pretty~ is a compile time information'

// ---cut---

let a: pretty;
//  ^?

// comments refer to things that happen at runtime

console.log('hello');
// > 'hello'

```

////

## Runtime types

types returned by `typeof`
```js
const a = 5;
typeof a; // returns 'number'
```

////

## Compile-time types

what your IDE shows you

```ts twoslash
let a = 5;
//  ^?
typeof a; // returns 'number'
```

////

can be more generic or specific than the runtime type

compile time type information generally not available at runtime

///

# Explicit type declaration

////

```ts twoslash
// @errors: 2322
let a: number;

a = 5;
```

////

```ts twoslash
function multiply(x: number, y: number): number {
    return x * y;
}
```

///

# More generic types

////
## `any`

```ts twoslash
let a: any;

a = 5;
typeof a; // returns 'number'

a = 'hello';
typeof a; // returns 'string'
```

////

## `any`
* most generic type that will be inferred
* handling correctly is up to you!

////

```ts twoslash
let a: any;

a = 5;

a.someFunction(); // Uncaught TypeError:
                  // a.someFunction is not a function
```

^ that's a *runtime error*!

////

## `unknown`

```ts twoslash
// @errors: 2571
let a: unknown;

a = 5;

a.someFunction(); 
```

////

## type union

```ts twoslash
let a: number|string;

a = 5;
typeof a; // returns 'number'

a = 'hello';
typeof a; // returns 'string'

```

////

## type narrowing
* flow control analysis
* compiler *understands* what runtime code means for types

////

```ts twoslash
function foo(a: number|string) {
    if (typeof a === 'number') {
        a;
    //  ^?
    } else {
        a;
    //  ^?
    }
}
```

///

# More specific types

////
# literal types

```ts twoslash
let a: 'up';

a = 'up';
```

////

???

```ts twoslash
// @errors: 2322
let a: 'up';

a = 'up';

a = 'hello';
```

////

```ts twoslash
// @errors: 2820
type Direction = 'up'|'down'|'left'|'right';

let a: Direction = 'up';

a = 'down';

a = 'rihgt';
```

////

more specific than runtime type
```ts twoslash
type Direction = 'up'|'down'|'left'|'right';

let a: Direction = 'up';

typeof a; // returns 'string'
```

///

# Interfaces

////

```ts twoslash
interface Cat {
    name: string;
    meow: () => string;
}
```

////

```ts twoslash
// @noErrors
interface Cat {
    name: string;
    meow: () => string;
}

let m: Cat;

m.name;
//^?
```

////
## structural typing
* types compatible? check structure of members!

////

```ts twoslash
interface Cat {
    name: string;
    meow: () => string;
}

let m: Cat;

m = {
    name: 'Minky',
    meow: () => 'nyaaa!',
}
```
object is cat-shaped, so we can use it as a cat

////

```ts twoslash
// @errors: 2322
interface Cat {
    name: string;
    meow: () => string;
}

let m: Cat;

m = {
    name: 'Fetch',
    bark: () => 'woof!',
}
```

////

* nice: saves lots of explicit type declarations
* works well with javascript concepts

* not so nice: more types compatible than you might want

Note:

using library: no need to explicitly import interfaces

////

```ts twoslash
interface Cake {
    name: string;
    ingredients: string[];
}

interface Bread {
    name: string;
    ingredients: string[];
}
```


////
```ts twoslash
interface Cake {
    name: string;
    ingredients: string[];
}

interface Bread {
    name: string;
    ingredients: string[];
}


/// ---cut---

function eatCake(c: Cake) {

}

const b: Bread = {
    name: 'wholegrain',
    ingredients: ['whole-wheat flour', 'wheat bran', 'yeast']
}

eatCake(b);
```

///

# Advanced types

////

## Discriminated Union

////

```ts twoslash
interface Cake {
    kind: 'cake',
    name: string;
    ingredients: string[];
}
interface Bread {
    kind: 'bread',
    name: string;
    ingredients: string[];
}

type BakedGood = Cake|Bread;
```

////

```ts twoslash
interface Cake {
    kind: 'cake',
    name: string;
    ingredients: string[];
}
interface Bread {
    kind: 'bread',
    name: string;
    ingredients: string[];
}

// ---cut---

type BakedGood = Cake|Bread;

const c: Cake = {
    kind: 'cake',
    name: 'Chocolate Cake',
    ingredients: ['flour', 'cocoa', 'chocolate']
}

```

////

```ts twoslash
interface Cake {
    kind: 'cake',
    name: string;
    ingredients: string[];
}
interface Bread {
    kind: 'bread',
    name: string;
    ingredients: string[];
}

// ---cut---

type BakedGood = Cake|Bread;

function eat(b: BakedGood) {
    if (b.kind === 'bread') {
        b;
//      ^?
    } else {
        b;
//      ^?
    }
}

```

////

* add information that is available at run-time
* compiler can use to infer overall type
* flow control analysis


////

## Template literal types

```ts twoslash
// @errors: 2322
type Unit = 'px'|'em'|'%';

type Measurement = `${number}${Unit}`;

const width: Measurement = '1px';
```

////


```ts twoslash
// @errors: 2322
type Unit = 'px'|'em'|'%';

type Measurement = `${number}${Unit}`;

const height: Measurement = '42';
```

///

# Takeaway
* powerful, expressive type system
* odd corners owed to javascript
* can greatly improve code safety and clarity

Note:

* architecture, type declarations inform how much the compiler can help you
