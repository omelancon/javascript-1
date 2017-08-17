# ALDO Functional Javascript Style Guide
- [Motivation](#motivation)
- [Values](#values)
- [1.0 Architecture](#10-architecture)
  * [1.1 A Note About Reusability](#11-a-note-about-reusability)
  * [1.2 Separation of concerns based on business requirements](#12-separation-of-concerns-based-on-business-requirements)
  * [1.3 Utils module and utils folders](#13-utils-module-and-utils-folders)
  * [1.4 Helpers and utils with business logic](#14-helpers-and-utils-with-business-logic)
- [2.0 React](#20-react)
- [3.0 Style Guide](#30-style-guide)
  * [3.1 Point-Free Style](#31-point-free-style)
  * [3.2 One Input, One Output](#32-one-input-one-output)
  * [3.3 Single Returns](#33-single-returns)
  * [3.4 Function Declarations](#34-function-declarations)
  * [3.5 Function Keyword](#35-function-keyword)
  * [3.6 Arrow Functions : is the default, noop, identity, always](#36-arrow-functions--is-the-default-noop-identity-always)
    + [Always](#always)
    + [Identity](#identity)
    + [Noop](#noop)
  * [3.7 Variable Declaration](#37-variable-declaration)
  * [3.8 Destructuring](#38-destructuring)
  * [3.9 Document Impurity](#39-document-impurity)
  * [3.10 Avoid Curly Braces for Code Blocks](#310-avoid-curly-braces-for-code-blocks)
  * [3.11 Keep Variables Close to Usage](#311-keep-variables-close-to-usage)
  * [3.12 Regex](#312-regex)
- [4.0 Control Flow](#40-control-flow)
  * [4.1 Monads](#41-monads)
    + [Examples](#examples)
  * [4.2 Do Not Use `null` & `undefined` for Control Flow](#42-do-not-use-null--undefined-for-control-flow)
  * [4.3 Asynchronous](#43-asynchronous)
  * [4.4 Avoid If Expressions](#44-avoid-if-expressions)
  * [4.5 Prefer Ternary Expression Over || and &&](#45-prefer-ternary-expression-over--and-)
  * [4.6 Use Ternary Expression to Determine data/functions Rather Than Execution](#46-use-ternary-expression-to-determine-datafunctions-rather-than-execution)
- [5.0 Composable Functions](#50-composable-functions)
  * [5.1 Impure Dependencies Must be Composed - Pure Functions](#51-impure-dependencies-must-be-composed---pure-functions)
- [6.0 Declarative Programming](#60-declarative-programming)
  * [6.1 Program Declarative Functions](#61-program-declarative-functions)
  * [6.2 Split Code Into Composable Functions](#62-split-code-into-composable-functions)
- [7.0 State & Function Purity](#70-state--function-purity)
  * [7.1 Assignments & State Modification](#71-assignments--state-modification)
  * [7.2 Object Assign](#72-object-assign)
  * [7.3 Do Not Share State](#73-do-not-share-state)
  * [7.4 Local State Mutation](#74-local-state-mutation)
  * [7.4 New Keyword](#74-new-keyword)
  * [7.5 This Keyword](#75-this-keyword)
- [8.0 Loops](#80-loops)
  * [8.1 For, While, Foreach](#81-for-while-foreach)
  * [8.2 Recursion & Tail Call](#82-recursion--tail-call)
- [9.0 Function Type Signature Documentation (Hindley-Milner)](#90-function-type-signature-documentation-hindley-milner)
  * [9.1 Parentheses in Hindley-Milner](#91-parentheses-in-hindley-milner)
- [10.0 Partial Evaluation](#100-partial-evaluation)
  * [10.1 A Word On Partial Application](#101-a-word-on-partial-application)
  * [10.1 More Than Just Application](#101-more-than-just-application)
  * [10.2 Usage](#102-usage)
    + [When To Use](#when-to-use)
    + [When Not To Use](#when-not-to-use)
- [11.0 Constants](#110-constants)
  * [11.1 Configurations](#111-configurations)

# Motivation
The functional javascript style guide was created to scope what can be done with javascript to promote a functional codebase. At ALDO, we believe that high-level programming language like javascript benefits more from the maintainability of the functional style than an imperative performance focused style.

This guide aims not to restrict the style of individual contributors but set the common ground for solving problems as a team with the javascript language.

This guide is opinionated, it is not the *ideal* or *better* way of programming in javascript nor programming Functional javascript, it is an idea of how it may be done. Anyone may use it, we at ALDO feel it's the better way to do it.

You may use it, but at your own risks. We open source this guide in hopes it helps other derive work from it or simply use it and maybe help us improve it.

# Values
These are the values we uphold with this guide and hope that this will help at releasing better software more regularly and steadily.

- Functional is the only way
- Impurity must be rejected
- Everything must be composable
- Monads for control flow

# 1.0 Architecture
A well architectured' javascript program is done through the module architectural pattern. Which decouple modules (components) and expose composable interfaces. Therefore it is paramount to properly apply the module pattern.

So what is a module? It is a self-contained program, that takes an input and return an output. This program can be pure or impure. But a pure program cannot receive an impure input. So expect inputs to be data. This program can have any umber of private function to achieve its goal. But as a general idea, it should have only one interface -- one export.

A module can require or import as many modules it needs to achieve its goal. But you should try to keep the module small and centered around one responsibility.

A module should be in its own file, and all of its sub-modules in the same directory. In this manner, a composed module can be replaced or deleted from the javascript program without any major side effects.

One could say that a complex javascript program is a collection of modules that could be replaced with a module with the same public interface.

## 1.1 A Note About Reusability
Opposite to what some people may think, the idea of reusability is not so much a goal but a side effect in functional programming. It's something you attain because you design the components properly, not because you avoid copying every piece of code. We much rather see code, that will never change because it has only one responsibility, be copied over as a private function in another module than to be pushed as a utility or its own module. Moreover, think like this, copy the code first, use unit tests to document the functionality, globalization of the code is the last resort thing.

## 1.2 Separation of concerns based on business requirements
Directories represent modules in a javascript program, so they should not be organized around technical concerns but centric to a business concern and separated as such.
``` javascript
// bad
_ src
├── actions
|   └── index.js
├── components
|   └── apple.js
|   └── microsoft.js
├── containers
|   └── apple.js
|   └── microsoft.js
├── reducers
|   └── apple.js
|   └── microsoft.js
├── sagas
|   └── apple.js
|   └── microsoft.js
└── index.html

// good
_ src
├── apple
|   └── apple.action.js
|   └── apple.component.js
|   └── apple.container.js
|   └── apple.saga.js
|   └── apple.gtm.js
├── microsoft
|   └── microsoft.action.js
|   └── microsoft.component.js
|   └── microsoft.container.js
|   └── microsoft.saga.js
|   └── microsoft.gtm.js
└── index.html
```

## 1.3 Utils module and utils folders
Do not use utils folder there you put the non business logic code. This couples the codebase uselessly and increases its own complexity. Rather promote utils to an npm repository. If the usage is common enough to be on npm it could already be there or you will serve the nodejs open source community.

## 1.4 Helpers and utils with business logic
Do not use the helper naming and concept. A function has only one purpose and its name should represent that. Moreover, it should be private to its module. Although it may cause code duplication, it will decrease complexity by decoupling modules.

# 2.0 React
We build React the AirBnb way [https://github.com/airbnb/javascript/tree/master/react].

# 3.0 Style Guide
This document is a collection of directives to better design code in Functional Javascript. But more directly [next-format](https://www.npmjs.com/package/next-format) is the pretty-printer we chose to enforce the way of laying out our javascript; it replaces the usual linter and rules that are up for interpretations and discussions. All the code must be next-formatted.

## 3.1 Point-Free Style
You should favor Point-Free Style [(tacit programming)](https://en.wikipedia.org/wiki/Tacit_programming), where you do not name the data you are operating on.
``` javascript
// bad
const do = shoe => selectDiscount(selectPriceObject(shoe))

// good
const do = compose(selectDiscount, selectPriceObject)
```

## 3.2 One Input, One Output
``` javascript
// bad
const add = (a, b) => a + b

const foo = (a, b, c, d, e) => (/* ... */)
const foo = a => b => c => d => e => (/* ... */)

// ok
const add = ({a, b}) => a + b
const add = ([a, b]) => a + b

// best
const add = a => b => a + b
```

## 3.3 Single Returns
``` javascript
// bad
const foo = a => {
  if (!a){
    return bar(0)
  }
  return bar(a)
}

// ok
const foo = a => bar(a ? a : 0)

// best
const foo (a = 0) =>  bar(a)
```

## 3.4 Function Declarations
You should avoid function declaration and use function expression.
``` javascript
// bad
function foo() { ... }

// good
const foo = () => ...

// generators
// bad
function* foo() { ... }

// good
const foo = function* () { ... }

// async functions
// bad
async function foo () { ... }

// good
const foo = async () => { ... }
```

## 3.5 Function Keyword
You should favor arrow functions over the function keyword.
``` javascript
// bad
const foo = function() { ... }

// good
const foo = () => ...
```

## 3.6 Arrow Functions : is the default, noop, identity, always
You should always use array function if you can. It's shorter more concise lambda and does not come with an implicit state but it's lexical context.

### Always
```javascript
// bad
function () { return 'Hello World' }

// good
() => 'Hello World'
```

### Identity
```javascript
// bad
function (a) { return a }

// good
a => a
```

### Noop
```javascript
// bad
function () { }

// good
() => { }
```

## 3.7 Variable Declaration
There can be just one: `const`. You should never use `var` and avoid at all cost `let`.

## 3.8 Destructuring
Although destructuring is a cool tool in es6, it binds the function to the json structure.
``` javascript
// bad
const parse = ({ a : { b } }) =>
  Id(b).map(parseInt).fold(a => a)

// good
const parse = path => data =>
  Id(data).map(path).map(parseInt).fold(a => a)

// best
const parse = path => compose(parseInt, path)
```

## 3.9 Document Impurity
Impure functions or impure function groups must be commented with `IMPURE` the same can be done for pure functions `PURE`.
``` javascript
// PURE
const upper = a => s.toUpperCase()
const selectBody = res => res.body

// IMPURE
const requestBodyToUpperCase = compose(upper, selectBody, getHttp)
```

## 3.10 Avoid Curly Braces for Code Blocks
Clutter, they are just clutter. You better use sequences. If you need curly braces, your function is probably too big, has multiple concerns or is not built properly.
``` javascript
// bad
const incremnt = a => {
  return a + 1
}

// bad
const make = flower => color => {
  flower(color)
  return color
}

// good
const incremnt = a => a + 1

// good
const make = flower => color => (flower(color), color)
```

## 3.11 Keep Variables Close to Usage
Keep variables closer to usage, inside function block if possible and just prefer using string literals.
``` javascript
// bad
const INC = 2
...
const increment = a => a + INC

// bad
const indcremnt = a => {
  const INC = 2
  return a + INC
}

// good
const indcremntBy2 = a =>  a + 2
```

## 3.12 Regex
Because a regex is a business case, you should use it as a string literal inside a function that does just that.
Avoid the `new Regex` construct.
``` javascript
// bad
const TRIM_END = /[ ]+$/
...
const trim = str => str.trim(TRIM_END)

// bad
const trim = str => str.trim(new Regex(/[ ]+$/)))

// good
const trim = str => str.trim(/[ ]+$/)
```

# 4.0 Control Flow

> Category theory gives you a framework for thinking about functions, composition, types, & abstraction. We should rename it to program theory

-- [Brian Lonsdorf](https://twitter.com/drboolean/status/886975835084804097)

## 4.1 Monads
You should use monads for control flow.
``` javascript
// bad
const makeFoo = foo => {
  if (foo) {
    return make(foo)
  }
}

// good
const makeFoo =
  foo =>
    Either.fromNullable(foo)
      .fold(make)
```
### Examples
``` javascript
Id(5)
  .map(num => num * 7)
  .map(num => num - 1)
  .fold(log)
//=> 34
```

``` javascript
// Maybe of a string
Maybe('Hello exalted one')
  .map(sentence => sentence.toUpperString())
  .map(sentence => `${sentence}!`)
  .fold(log)
//=> 'HELLO EXALTED ONE!'

// Maybe of nothing
Maybe(null)
  .map(sentence => sentence.toUpperString())
  .else(() => 'Maybe received a null')
  .fold(log)
//=> 'Maybe received a null'
```

``` javascript
Either.fromNullable('Hello') // this will return a Right('Hello')
  .fold(
    () => 'Oops',
    val => `${val} world!`)
//=> 'Hello world!'

Either.fromNullable(null) // this will return a Left(null)
  .fold(
    () => 'Oops',
    val => `${val} world!`)
//=> 'Oops'

const extractEmail = obj => obj.email ? Right(obj.email) : Left()
extractEmail({ email: 'test@example.com' })
  .map(extractDomain)
  .fold(
    () => 'No email found!',
    x => x)
//=> 'example.com'

extractEmail({ name: 'user' })
  .map(extractDomain) // this will not get executed
  .fold(
    () => 'No email found!',
    x => x)
//=> 'No email found!'
```

``` javascript
List([2, 4, 6])
  .map(num => num * 2)
  .filter(num => num > 5)
  .fold(log)
//=> [8, 12]
```

## 4.2 Do Not Use `null` & `undefined` for Control Flow
Control flow should be done through monads.
``` javascript
// bad
const getBody = document => document && document.body ? document.body : undefined
const getBody = document => document && document.body

// good, a monadic api is more reliable and defers the decisions to the caller
const getBody = document =>
  Either.fromNullable(document)
  .map(d => d.body)

// good
const getBody = document =>
  Either.fromNullable(document)
  .map(d => d.body)
  .fork(() => 'can\'t get body of null', b => b)
```

## 4.3 Asynchronous
Because we favor monadic workflow asynchronous processing should be encapsulated in the `Future` monad.

``` javascript
// Basic usage
Future((reject, resolve) => resolve('Yay'))
  .map(res => res.toUpperString())
  .fork(
    err => log(`Err: ${err}`),
    res => log(`Res: ${res}`))
//=> 'YAY'

// Handle promises
Future.fromPromise(fetch('https://api.awesome.com/catOfTheDay'))
  .fork(
    err => log('There was an error fetching the cat of the day :('),
    cat => log('Cat of the day: ' + cat))
//=> 'Cat of the day: Garfield'

// Chain http calls
Future.fromPromise(fetch('https://api.awesome.com/catOfTheDay'))
  .chain(cat => Future.fromPromise(fetch(`https://api.catfacts.com/${cat}`)))
  .fork(
    err => log('There was an error fetching the cat of the day :('),
    facts => log('Facts for cat of the day: ' + facts))
//=> 'Facts for cat of the day: Garfield is awesome.'
```

## 4.4 Avoid If Expressions
Do not use if expression, they influence imperative programming.
``` javascript
// bad
(foo) => {
  if (foo) {
    return bar
  }
  return baz
}

// good
(foo) => foo ? bar : baz
```

## 4.5 Prefer Ternary Expression Over || and &&
Prefer the usage of ternary expression where you are explicit of the else condition.
``` javascript
// ok
(foo) => foo && bar || 0

// good
(foo) => foo ? bar : { baz: true }
(foo) => foo ? baz : 0
```

## 4.6 Use Ternary Expression to Determine data/functions Rather Than Execution
``` javascript
// bad
const foo = data => make => make2 => condition => condition ? make(data) : make2(data)

// good
const foo = make => make2 => condition => condition ? make : make2
foo(make)(make2)(condition)(data)
```

# 5.0 Composable Functions

## 5.1 Impure Dependencies Must be Composed - Pure Functions
``` javascript
// bad
const readFileBody = filePath => {
  const fileContents = readFileSync(filePath)
  const json = JSON.parse(fileContents)
  return json.body
}

// good
const getBody = document => document.body
const readFileBody = compose(getBody, JSON.parse, readFileSync)
```

# 6.0 Declarative Programming
## 6.1 Program Declarative Functions
Because you should tell a story by declaring what to do and not how you should avoid imperative functions that tend to tell the computer how to do the thing rather than declare what to do.
``` javascript
// bad
function fromPairs(pairs) {
  var result = {}
  var idx = 0
  while (idx < pairs.length) {
    result[pairs[idx][0]] = pairs[idx][1]
    idx += 1
  }
  return result
}

// good
const fromPairs = compose(combine, map(([key, value]) => ({ [key]: value })))
```

## 6.2 Split Code Into Composable Functions
``` javascript
// bad
const splitToKeyValuePair = headerString => {
  return headerString.split(',')
    .reduce((result, current) => {
      const keyValuePair = current.split('=')
      const key = keyValuePair[0]
      const value = keyValuePair[1]
      result[key] = value
      return result
    }, {})
}

// good
const splitToKeyValuePair = compose(combine, fromPairs, map(split('=')), map(trim), split(','))
```

# 7.0 State & Function Purity
## 7.1 Assignments & State Modification
As a general rule you should avoid assignments at all costs, they alter state and increase the risk of sharing state in code and changing functions parameters.
``` javascript
// bad
const foo = state => {
  state.count = state.count + 1
  return state
}

// good
const foo = state => object.assign({}, state, { count: state.count + 1 })

// best
const foo = state => ({ ...state, count: state.count + 1 })
```

## 7.2 Object Assign
Use `Object.assign` to avoid mutating state.
``` javascript
// bad
const state => {
  state.id = 1
  return state
}

// good
const state => Object.assign({}, state, { id : 1 }) // native js

const state => ({ ...state, id : 1 }) // es6 syntax
```

## 7.3 Do Not Share State
``` javascript
// bad
const inc = () => state.count++

// good
const inc = (state = { count: 0 }) => ({ count : state.count + 1 })
```

## 7.4 Local State Mutation
You should try to avoid mutating local state. The inconvenience with doing it is that you might inadvertently cause a side effect by mutating a function parameter.
``` javascript
// bad
const fiftyPercentDiscount = shoe => {
  shoe.price + shoeVariant.price / 2
  return shoe
}

// good
const fiftyPercentDiscount = shoe => ({ ...shoe, price: shoe.price / 2 })
```

## 7.4 New Keyword
Never use the new keyword.

## 7.5 This Keyword
Because we code pure function and avoid shared state we must not use the `this` keyword, ever.

# 8.0 Loops

## 8.1 For, While, Foreach
Loops are inherently imperative. They also mix concerns: iteration and execution are two different concerns that, if handled separately from each other, result in more flexible code.

Better options include:

* [recursion](#recursion--tail-call)
* map
* filter
* reduce

## 8.2 Recursion & Tail Call
To make a tail call recursion you need to place the function call at the end of your function and have it return the value. [see es6-recursion-tail-recursion](http://www.door3.com/insights/es6-recursion-tail-recursion)
``` javascript
// recur :: Number -> Number -> Number
const recur = n => acc =>  n == 0 ? acc : recur(n-1)(n * acc)

// recur :: Number -> Number
const factorial = (n) => recur(n)(1)
```

# 9.0 Function Type Signature Documentation (Hindley-Milner)
You should document all functions with Hindley-Milner annotation, it is the prevalent type signature documentation in functional languages (Haskell, Elm, OCaml, etc.) and it just makes a lot of sense to use it in functional Javascript.
```
// functionName :: type -> type -> type

// add :: Number -> Number -> Number
const add = x => y => x + y

// concat :: [a] -> [a] -> [a]
const concat = a => b => a.concat(b)

// filter :: (a -> Boolean) -> [a] -> [a]
const filter = f => a => a.filter(f)

// map :: (a -> b) -> [a] -> [b]
const map = f => a => a.map(f)

// replace :: String -> String -> String -> String
const replace = subStr => newSubstr => s => s.replace(subStr, newSubstr)

// uncurriedMaxOfThree :: (Number, Number, Number) -> Number
uncurriedMaxOfThree (x, y, z) = Math.max(x, y, z)

// curry :: ((a, b) -> c) -> a -> b -> c
const curry = f => x => y => f(x, y)

// uncurry :: (a -> b -> c) -> (a, b) -> c
const uncurry => f => (x, y) =>  f(x)(y)

// returnOne :: () -> Number
const returnOne = () => 1
```

### Notation for Arrays

As seen above, we do not use `Array` in function signatures, even though this is a proper type in Javascript. Instead we use the square bracket notation `[type]` to indicate, not only that we expect an array, but to indicate the type of the elements it contains.

```
// sum :: [Number] -> Number
const sum = numberArray => numberArray.reduce((x, y) => x + y, 0)

// notation for an array of Numbers: [Number]
// notation for an array of Strings: [String]
// notation for a two-dimensional array of numbers: [[Number]]
```

### Polymorphic Types

A polymorphic function is a function which has at least one argument that can be of any type. To indicate a polymorphic type, we denote it by a variable.

```
// The identity function is polymorphic, but its return type matches its argument type

// id :: a -> a
const id = x => x

// The second argument of repeat is of any type, but it will return an array of the same type

// repeat :: Number -> a -> [a]
const repeat = n => a => n < 1 ? [] : [a].concat(repeat(n - 1)(a)) 

// The function flip has polymorphic types, but the notation allows us to read which types should match

// flip :: (a -> b -> c) -> b -> a -> c
const flip = f => b => a => f(a)(b)
```


### Notation for Objects

Notice that the type `Object` is sometimes not meaningful enough since we might require that an object has some specific attribute.

```
// for a merge function it is accurate to us Object as type

// merge :: Object -> Object -> Object
const merge = x => y => Object.assign({}, x, y)

// for a function that needs to access a specific attribute of the object, type Object becomes inadequat

// selectPersonName :: Object -> String
const selectPersonName = obj => obj.name
```

To be more explicit, we replace `Object` by a name that accurately represents what the object structure is expected to be.

```
// bad
// update :: Object -> Object
 const selectAccountSummary = a => ({
    income: a.income,
    expense: a.expense,
    balance: a.income - a.expense
 })

// good
// update :: Account -> AccountSummary
 const selectAccountSummary = a => ({
    income: a.income,
    expense: a.expense,
    balance: a.income - a.expense
 })
```

If renaming `Object` is not clear enough in the context, it is possible to add a comment defining the expected structure of the object.

#### Caveat

The purpose of renaming the `Object` type is to give information about its structure and the attributes it contains. It should not be done to define what the object represents. Thus, renaming should not be used for types other than `Object`, because their structure is already well-defined. Furthermore renaming non-object types can create confusion.

```
// bad

// Here, renaming Number to Price may lead to think Price is an Object and not a Number

// priceToString :: Price -> Price as String
const priceToString = price => price + '$'

// good
// priceToString :: Number -> String
const priceToString = price => price + '$'
```


## 9.1 Parentheses in Hindley-Milner
Parentheses in Hindley-Milner notation can play three roles, of which only two are mandatory.

First of all, parentheses will denote tuples of arguments for uncurried functions. Arguments as tuples indicate that the function cannot be partially applied. In functional Javascript, you generally want to avoid uncurried functions.

```
// uncurriedAdd :: (Number, Number) -> Number
const uncurriedAdd = (x, y) -> x + y

// This will not partially evaluate, but return NaN instead
uncurriedAdd(1)
```

A common use of parentheses is also to enclose functions, i.e. to state that the argument you expect is a function with the given enclosed signature.

```
// The first argument is expected to be of type (a -> b)
// so it would be incorrect to omit the parentheses.

// apply :: (a -> b) -> a -> b
const apply = f => x => f(x)
```

Finally, while this final case is optional, parentheses can be used to encourage the user to partially apply a function. Indeed some functions such as `curry` are rarely, if ever, fully evaluated.

```
// Both signatures are correct, but the second one indicates to the user
// that curry should be partially applied to return a function to be reused later

// curry :: ((a, b) -> c) -> a -> b -> c
// curry :: ((a, b) -> c) -> (a -> b -> c)
const curry = f => x => y => f(x, y)

// The end parentheses are optional since a -> b -> c is equivalent to (a -> b -> c)
```

# 10.0 Partial Evaluation

## 10.1 A Word On Partial Application

Partial application is when you give some of its arguments to a function to return a function that takes fewer arguments. The process of making a function partially applicable is also referred as currying.

```
// add :: Number -> Number -> Number
const add = x => y => x + y

// addTwo is a partial application of add

// addTwo :: Number -> Number
const addTwo = add(2)

addTwo(5) //=> 7
```

## 10.1 More Than Just Application

Always keep in mind that partial application of a function does not partially evaluate it. All computations, however heavy they may be, are executed only once all arguments have been provided. Partial evaluation is a good way to improve partially applied functions by precomputing as much as possible with the given arguments.

```
// This function can be partially applied, but is not partially evaluated
// addThenMultiply :: Number -> Number -> Number -> Number
const addThenMultiply = x => y => z => (x + y) * z

// This function is partially evaluated when partially applied
// betterAddThenMultiply :: Number -> Number -> Number -> Number
const betterAddThenMultiply = x => y => {
  const value = x + y
  return z => value * z
}

// multiplyFour will unnecessarily compute 2 + 2 everytime it is called
// multiplyFour :: Number -> Number
const multiplyFour = addThenMultiply(2)(2)

// betterMultiplyFour already computed 2 + 2
// betterMultiplyFour :: Number -> Number
const betterMultiplyFour = betterAddThenMultiply(2)(2)
```

The purpose of the previous example is to introduce the concept of partial evaluation. Although, this example is hardly faster in practice since saving a single arithmetic operation is negligible. Let's consider a more meaningful example where partial evaluation gives a significant advantage.

```
// filterArraysThenConcat :: (a -> Boolean) -> [a] -> [a] -> [a]
const filterArraysThenConcat = myFilter => firstArray => {
  const filteredFirstArray = firstArray.filter(myFilter)
  return secondArray => filteredFirstArray.concat(secondArray.filter(myFilter))
}

const myFavouriteNumbers = [1, 2, 3, ..., 999999, 1000000]
const evenFilter = n => n % 2 === 0

// extendMyArrayOfEvenNumbers :: [Number] -> [Number]
const extendMyArrayOfEvenNumbers = filterArraysThenConcat(evenFilter)(myFavouriteNumbers)

// This call is way more efficient since the first filter has already been applied
extendMyArrayOfEvenNumbers([-1, -2, -3])
```

## 10.2 Usage

Partial evaluation is a useful optimization tool when used properly. Its main downside is readability, observe that we had to break our [rule about curly braces](https://github.com/aldo-dev/javascript#avoid-curly-braces-for-code-blocks) in the process. For that reason it should not be systematically used.

### When To Use

- Partially applied functions that you intend to reuse a lot
- Partially applied functions which execution can be arbitrarily long (ex: operations on huge Arrays)

### When Not To Use

- When readability becomes an issue, maybe write smaller composable functions
- Never use partial evaluation for impure functions
- Whenever it is not absolutely necessary!


# 11.0 Constants
You may use string, boolean or number literals. But if you use constants, they must be close to usage, part of the module that uses them and in no other event and configuration in a different file than where they are used.

``` javascript
// good
const userCountReducer = (userCount = 5) => ...

// good
const USER_DEFAULT_COUNT = 5
const userCountReducer = (state = USER_DEFAULT_COUNT) => ...

// good
const USER_DEFAULT_COUNT = 5
const fromDefaults = ()=>  ({ userCount: USER_DEFAULT_COUNT })

// good
const fromDefaults = () => ({ userCount: 5 })
```

## 11.1 Configurations
Constants are often used to implement configuration - parameters to the program as a whole that need to be changeable independently of code. In these cases, globality is more reasonable; however you should still try to think of ways to make configuration specific to the module you're working on.
``` javascript
// good
const { USER_DEFAULT_COUNT } = require('./configuration')
... // a few lines doen but not too far
const userCountReducer = (state = USER_DEFAULT_COUNT) => ...
```

``` javascript
// bad
const USER_DEFAULT_COUNT = 5
...
const reducer = (state = USER_DEFAULT_COUNT) => ... // you just can't know that the default is 5, it makes no sense

// good
const reducer = (state = 5) => ...
```
