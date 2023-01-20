Notes from [Mike North](https://github.com/mike-north) Course on [TypeScript fundamentals](https://frontendmasters.com/courses/typescript-v3/)
- https://www.typescript-training.com/course/fundamentals-v3
- https://www.typescriptlang.org/

# Introduction

Basic TS program contains the following files:
```
package.json   # Package manifest
tsconfig.json  # TypeScript compiler settings
src/index.ts   # "the program"
```

# Variable and Values
**In TypeScript, variables are “born” with their types.**
```ts
let age: number = 6
const age: number = 6 // constant variable declarations cannot be reassigned
let endTime: Date // only add when I have to
```

# Functions
```ts
// functon
function add(a: number, b: number): number {}

// documenation comment
/**
	*
	* @param a {number}
	*
	*/
```

# Collections
## Objects
In general, object types are defined by:

-   The **names** of the properties that are (or may be) present
-   The **types** of those properties

```ts
// car object
const car = {
  make: "Toyota",
  model: "Corolla",
  year: 2002
}

// type
{
  make: string
  model: string
  year: number
}
```

**Optional properties and Type guards**
- optional properties can be left out
```ts
function printCar(car: {
  make: string
  model: string
  year: number
  chargeVoltage?: number //optional
}) {
  let str = `${car.make} ${car.model} (${car.year})`
  car.chargeVoltage

  //! Type guard
  if (typeof car.chargeVoltage !== "undefined")
    str += `// ${car.chargeVoltage}v`
}
```

**Excess Property Checking**
![](../Attachments/Pasted%20image%2020230119151119.png)
Four ways to fix this
1.  Remove the `color` property from the object
2.  Add a `color?: string` to the function argument type
3.  Create a variable to hold this value, and then pass the variable into the `printCar` function
![400](../Attachments/Pasted%20image%2020230119151534.png)

### Index signatures
Sometimes we need to represent a type for **dictionaries**, where values of a consistent type are retrievable by keys.

Let’s consider the following collection of phone numbers:
![](../Attachments/Pasted%20image%2020230119151939.png)

Clearly it seems that we can store phone numbers under a “key”
- in this case `home`, `office`, `fax`, and possibly other words of our choosing
- Each phone number is comprised of three strings.

![](../Attachments/Pasted%20image%2020230119152051.png)

## Array
```ts
// simple array
const myNumbers: number[]
myNumbers = [1,2,3,4,5]

// array of object
const arrayOfObj = {}[]
//example
const cars: {
    make: string;
    model: string;
    year: number;
}[];
```

## Tuples
Sometimes we may want to work with a multi-element, ordered data structure, where position of each item has some special meaning or convention. This kind of structure is often called a [tuple](https://en.wikipedia.org/wiki/Tuple)
```ts
// Correct way to do it
let myCar: [number, string, string] = [
	2002,
	"Toyota",
	"Corolla",
]

// ERROR: not the right convention
myCar = ["Honda", 2017, "Accord"]

// ERROR: too many items
myCar = [2017, "Honda", "Accord", "Sedan"]

// destructoring
const [year, make, model] = myCar
```

# Union and Intersection Types
Union and intersection types can conceptually be thought of as logical boolean operators (`AND`, `OR`) as they pertain to types.

### Union (OR)
A union type has [a very specific technical definition](https://en.wikipedia.org/wiki/Union_(set_theory)) that comes from set theory, but it’s completely fine to think of it as **OR, for types**.
- `"success" | "error"`
![300](../Attachments/Pasted%20image%2020230119154505.png)

We can use union to handle errors like this:
![](../Attachments/Pasted%20image%2020230119155151.png)

#### Narrowing with Type Guards
Ultimately, we need to “separate” the two potential possibilities for our value, or we won’t be able to get very far. We can do this with [type guards](https://www.typescriptlang.org/docs/handbook/2/narrowing.html).

> Type guards are expressions, which when used with control flow statement, allow us to have a more specific type for a particular value.

![](../Attachments/Pasted%20image%2020230119155801.png)

#### Narrowing with Discriminated unions
TypeScript understands that the first and second positions of our tuple are linked. What we are seeing here is sometimes referred to as a [discriminated or “tagged” union type](https://en.wikipedia.org/wiki/Tagged_union).

![](../Attachments/Pasted%20image%2020230119155958.png)

## Intersection (AND)
Intersection types also have [a name and definition that comes from set theory](https://en.wikipedia.org/wiki/Intersection_(set_theory)), but they can be thought of as **AND, for types**.
- It is _far_ less common to use intersection types compared to union types. I expect it to be at least a 50-to-1 ratio for you in practice.

![300](../Attachments/Pasted%20image%2020230119154416.png)

Intersection types in TypeScript can be described using the `&` (ampersand) operator.
![](../Attachments/Pasted%20image%2020230119160432.png)

# Type Aliases and Interfaces
TypeScript provides two mechanisms for centrally defining types and giving them useful and meaningful names: [interfaces](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#interfaces) and [type aliases](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#type-aliases).

## Type Aliases
Type aliases help to address this, by allowing us to:

-   define **a more meaningful name** for this type
-   declare the particulars of the type **in a single place**
-   **import and export** this type from modules, the same as if it were an exported value

```ts
type UserContactInfo = {
  name: string
  email: string
}
```


![](../Attachments/Pasted%20image%2020230119161112.png)

>It’s important to realize that the **name** `UserContactInfo` is just for our convenience. This is still a structural type system
>	- Does not matter if it has excess properties

![](../Attachments/Pasted%20image%2020230119161308.png)

![](../Attachments/Pasted%20image%2020230119163544.png)

### Inheritance in type aliases
- You can create type aliases that combine existing types with new behavior by using Intersection (`&`) types.
- While there’s no true `extends` keyword that can be used when defining type aliases, this pattern has a very similar effect

```ts
type SpecialDate = Date & { getReason(): string }
 
const newYearsEve: SpecialDate = {
  ...new Date(),
  getReason: () =%3E "Last day of the year",
}
newYearsEve.getReason
```

## Interfaces
An [interface](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#interfaces) is a way of defining an [_object type_](https://www.typescriptlang.org/docs/handbook/2/objects.html). An “object type” can be thought of as, “an instance of a class could conceivably look like this”.

```ts
interface UserInfo {
  name: string
  email: string
}
function printUserInfo(info: UserInfo) {
  info.name
}
```

### Inheritance in interfaces
#### Extends
-   Just as in in JavaScript, **a subclass `extends` from a base class**.
-   Additionally **a “sub-interface” `extends` from a base interface**, as shown in the example below

```js
// Animal in javascript
class Animal {
  eat(food) {
    consumeFood(food)
  }
}
class Dog extends Animal {
  bark() {
    return "woof"
  }
}
 
const d = new Dog()
d.eat
  
(method) Animal.eat(food: any): void
d.bark
```

```ts
// typescript interface
interface Animal {
  isAlive(): boolean
}
interface Mammal extends Animal {
  getFurOrHairColor(): string
}
interface Dog extends Mammal {
  getBreed(): string
}
function careForDog(dog: Dog) {
  dog.getBreed
}
```

#### Implements
![](../Attachments/Pasted%20image%2020230119170320.png)

#### Open Interfaces
TypeScript interfaces are “open”, meaning that unlike in type aliases, you can have multiple declarations in the same scope:

```ts
interface AnimalLike {
  isAlive(): boolean
}
function feed(animal: AnimalLike) {
  animal.eat
         
(method) AnimalLike.eat(food: any): void
  animal.isAlive
           
(method) AnimalLike.isAlive(): boolean
}
 
// SECOND DECLARATION OF THE SAME NAME
interface AnimalLike {
  eat(food): void
}
```

You may be asking yourself: **where and how is this useful?**
- Imagine a situation where you want to add a global property to the `window` object
![](../Attachments/Pasted%20image%2020230119170729.png)

## Choosing which to choose
In many situations, either a `type` alias or an `interface` would be perfectly fine, however…

1.  **If you need to define something other than an object type** (e.g., use of the `|` union type operator), you must use a type alias
2.  If you need to define a type **to use with the `implements` heritage term**, it’s best to use an interface
3.  If you need to **allow consumers of your types to _augment_ them**, you must use an interface.

# Recursion 
[Recursive types](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-1.html#recursive-conditional-types), are self-referential, and are often used to describe infinitely nestable types. For example, consider infinitely nestable arrays of numbers

![](../Attachments/Pasted%20image%2020230119171145.png)

# Advanced Functions

## Returns
Both type aliases and interfaces offer the capability to describe [call signatures](https://www.typescriptlang.org/docs/handbook/2/functions.html#call-signatures):
- The return value of a void function is intended to be _ignored_

```ts
// interface
interface TwoNumberCalculation {
  (x: number, y: number): number
}

// alias
type TwoNumberCalc = (x: number, y: number) => number

// undefined: saying I should always return undefined
function invokeInFourSeconds(callback: () => undefined) {
	setTimeout(callback, 4000)
}

// void: ignore the return type of this function (never used)
function invokeInFiveSeconds(callback: () => void) {
	setTimeout(callback, 5000)
}
```

## Function overloads
What if we had to create a function that allowed us to register a “main event listener”?

-   If we are passed a `form` element, we should allow registration of a “submit callback”
-   If we are passed an `iframe` element, we should allow registration of a ”`postMessage` callback”
![](../Attachments/Pasted%20image%2020230119172633.png)

This looks like three function declarations, but it’s really two “heads” that define an [argument list](https://262.ecma-international.org/#prod-ArgumentList) and a return type, followed by our original implementation.

## this types
Sometimes we have a free-standing function that has a strong opinion around what `this` will end up being, at the time it is invoked.
- For example, if we had a DOM event listener for a button:

```jsx
<button onClick="myClickHandler">Click Me!</button>
```

We could define `myClickHandler` as follows
![](../Attachments/Pasted%20image%2020230119173404.png)

# Class

## Constructors
In the TypeScript world, we want some assurance that we will be stopped at compile time from invoking the non-existent `activateTurnSignal` method on our car. In order to get this we have to provide a little more information up front:

![](../Attachments/Pasted%20image%2020230119175353.png)

Two things to notice in the code snippet above:

-   We are stating the types of each class field
-   We are stating the types of each constructor argument

## Access Modifier
TypeScript provides three **access modifier keywords**, which can be used
with class fields and methods, to describe **who should be able to see and use them**.

| keyword     | who can access                      |
| ----------- | ----------------------------------- |
| `public`    | everyone (this is the default)      |
| `protected` | the instance itself, and subclasses |
| `private`   | only the instance itself            |

![](../Attachments/Pasted%20image%2020230119180508.png)

## Param Properties
![](../Attachments/Pasted%20image%2020230119180848.png)

## Order of Operation OOP
Note the following order of what ends up in the class constructor:

1.  `super()`
2.  param property initialization
3.  other class field initialization
4.  anything else that was in your constructor after `super()`
![](../Attachments/Pasted%20image%2020230119181247.png)

# Types and Values
Types describe sets of allowed values

## Top Types
A [top type](https://en.wikipedia.org/wiki/Top_type) (symbol: `⊤`) is a type that describes **any possible value allowed by the system**. To use our set theory mental model, we could describe this as `{x| x could be anything }`

- `any` - “playing by the usual JavaScript rules”
	- appropriate to use `any` for `console.log()`
```ts
let flexible: any = 4
flexible = "Download some more ram"
flexible = window.document
flexible = setTimeout
```

- `unknown` - Like `any`, unknown can _accept_ any value:
	- Values with an `unknown` type cannot be _used_ without first applying a type guard

```ts
let myUnknown: unknown = 4

// This code runs for { myUnknown| anything }
if (typeof myUnknown === "string") {
	// This code runs for { myUnknown| all strings }
	console.log(myUnknown, "is a string")
	let myUnknown: string
} else if (typeof myUnknown === "number") {
	// This code runs for { myUnknown| all numbers }
	console.log(myUnknown, "is a number")
	let myUnknown: number
} else {
	// this would run for "the leftovers"
// { myUnknown| anything except string or numbers }
}
```

### Practical use of top types

You will run into places where top types come in handy _very often_. In particular, if you ever convert a project from JavaScript to TypeScript, it’s very convenient to be able to incrementally add increasingly strong types. A lot of things will be `any` until you get a chance to give them some attention.

`unknown` is great for values received at runtime (e.g., your data layer). By obligating consumers of these values to perform some light validation before using them, errors are caught earlier, and can often be surfaced with more context.

## Bottom type: `never`
A [bottom type](https://en.wikipedia.org/wiki/Bottom_type) (symbol: `⊥`) is a type that describes **no possible value allowed by the system**.
- TypeScript provides one bottom type: [`never`](https://www.typescriptlang.org/docs/handbook/2/functions.html#never).

![](../Attachments/Pasted%20image%2020230119182800.png)

I recommend handling this a little more gracefully via an **error subclass**:
![](../Attachments/Pasted%20image%2020230119182954.png)

## Type Guards 
There are a bunch of type guards that are included with TypeScript. Below is an illustrative example of a wide variety of them:
![](../Attachments/Pasted%20image%2020230119183414.png)

### User defined type guards
#### `value is Foo`
- The first kind of user-defined type guard we will review is an `is` type guard.
![](../Attachments/Pasted%20image%2020230119184016.png)

# Nullish Values

## null
- `null` means: there is a value, and that value is _nothing_.

```ts
const userInfo = {
	name: "Mike",
	email: "mike@example.com",
	secondaryEmail: null, // user has no secondary email
}
```

## undefined
- `undefined` means the value isn’t available (yet?)

```ts
const formInProgress = {
	createdAt: new Date(),
	data: new FormData(),
	completedAt: undefined, //
}

function submitForm() {
	formInProgress.completedAt = new Date()
}
```

## void
- `void` should exclusively be used to describe that a function’s return value should be ignored

```ts
console.log('console.log returns nothing')
```

# Generics

## Fundamentals

[Generics](https://www.typescriptlang.org/docs/handbook/2/generics.html) allow us to parameterize types, which unlocks great opportunity to reuse types broadly across a TypeScript project.

![](../Attachments/Pasted%20image%2020230119192817.png)

 The TypeParam, and usage to provide an argument type

```ts
const phoneList = [
  { customerId: "0001", areaCode: "321", num: "123-4566" },
  { customerId: "0002", areaCode: "174", num: "142-3626" },
  { customerId: "0003", areaCode: "192", num: "012-7190" },
  { customerId: "0005", areaCode: "402", num: "652-5782" },
  { customerId: "0004", areaCode: "301", num: "184-8501" },
]

// Generics
function listToDict<T>( // Parameterized in terms of T
	list: T[], // accept a list of T as the first argument
	idGen: (arg: T) => string // callback uses T as an arg and returns string
): { [k: string]: T }  // Arr. of T[] turned to obj. of T --> { [k: string]: T }
{
	const dict: { [k: string]: T } = {}
	
	list.forEach((element) => {
	    const dictKey = idGen(element)
	    dict[dictKey] = element
	})	
	
	return dict
}

const dict2 = listToDict(phoneList, (p) => p.customerId)
```

-   **`<T>` to the right of `listDict`**  
    means that the type of this function is now parameterized in terms of a type parameter `T` (which may change on a per-usage basis)
- **`list: T[]` as a first argument**  
means we accept a list of `T`‘s.
 >TypeScript will infer what `T` is, on a per-usage basis, depending on what kind of array we pass in. If we use a `string[]`, `T` will be `string`, if we use a `number[]`, `T` will be `number`.

-   **`idGen: (arg: T) => string`** is a callback that _also_ uses `T` as an argument. This means that…
    
    -   we will get the benefits of type-checking, within `idGen` function
    -   we will get some type-checking alignment between the array and the `idGen` function

More examples of Generics
![](../Attachments/Pasted%20image%2020230119204717.png)

## Generic Restraints

Generic constraints allow us to describe the “minimum requirement” for a type param, such that we can achieve a high degree of flexibility, while still being able to safely assume _some_ minimal structure and behavior.

- The way we define constraints on generics is by using the [`extends`](https://www.typescriptlang.org/docs/handbook/2/generics.html#generic-constraints) keyword.

![](../Attachments/Pasted%20image%2020230119205613.png)

Note that our “requirement” for our argument type (`HasId[]`) is now represented in two places:

-   `extends HasId` as the constraint on `T`
-   `list: T[]` to ensure that we still receive an array
- *I can be given any T but it has to at least meet this base requirements*

## Best Practices for Generics

-   **Use each type parameter _at least twice_**. Any less and you might be casting with the `as` keyword. Let’s take a look at this example:
