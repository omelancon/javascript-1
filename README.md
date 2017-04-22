# ALDO Javascript Functional Programming Guide
- [Architecture](/architecture)
- [Learning](/learning)
- [Process](/process)
- [Testing](/testing)

# Motivation
The javascript functional programming guide was created to scope what can be done with javascript to promote a functional codebase. At ALDO, we believe that high-level programming language like javascript benefits more from the maintainability of the functional style than an imperative performance focused style.

This guide aims not to restrict the style of individual contributors but set the common ground for solving problems as a team with the javascript language.

This guide is opinionated, it is not the *ideal* or *better* way of programming in javascript nor programming Functional javascript, it is an idea of how it may be done. Anyone may use it, we at ALDO feel it's the better way to do it.

You may use it, but at your own risks. We open source this guide in hopes it helps other derive work from it or simply use it and maybe help us improve it.

# Values
These are the values we uphold with this guide and hope that this will help at releasing better software more regularly and steadily.

- Functional is the only way
- Impurity must be rejected
- Everything must be composable
- Monads for control flow

# React
We build React the AirBnb way [https://github.com/airbnb/javascript/tree/master/react].

# Functional Programming the good, the bad, and you know what.

## Style Guide
This document is a collection of directives to better design code in Functional Javascript. But more directly [next-format](https://www.npmjs.com/package/next-format) is the pretty-printer we chose to enforce the way of laying out our javascript; it replaces the usual linter and rules that are up for interpretations and discussions. All the code must be next-formatted.

## Point-Free Style
You should favor Point-Free Style [(tacit programming)](https://en.wikipedia.org/wiki/Tacit_programming), where you do not name the data you are operating on.
``` javascript
// bad
const do = shoe => selectDiscount(selectPriceObject(shoe))

// good
const do = compose(selectDiscount, selectPriceObject)
```

## Functions Must be Shrinked
You should shrink the function to their simplest composable representation.
 *needs examples*

## Control Flow
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
*more examples needed*

*Future, Either, Maybe, List*

## Asynchronous
Because we favor monadic workflow asynchronous processing should be encapsulated in the `Future` monad.

*needs examples*

## Impure Dependencies Must be Composed - Pure Functions
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

## Do Not Share State - Pure Functions
``` javascript
// bad
const inc = () => state.count++

// good
const inc = (state = { count: 0 }) => ({ count : state.count + 1 })
```

## Local State Mutation
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

## Document Impurity
Impure functions or impure function groups must be commented with `IMPURE` the same can be done for pure functions `PURE`.
``` javascript
// PURE
const upper = a => s.toUpperCase()
const selectBody = res => res.body

// IMPURE
const requestBodyToUpperCase = compose(upper, selectBody, getHttp)
```

## Single Returns
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

## One Input, One Output
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

## Split Code Into Composable Functions
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

## Do Not Program Imperative Functions
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

## Do Not Use `null` & `undefined` for Control Flow
Control flow should be done through monads.
``` javascript
// bad
const getBody = document => document && document.body ? document.body : undefined
const getBody = document => document && document.body

// good, a monadic api is more reliable and differs the decisions to the caller
const getBody = document =>
  Either.fromNullable(document)
  .map(d => d.body)

// good
const getBody = document =>
  Either.fromNullable(document)
  .map(d => d.body)
  .fork(() => 'can\'t get body of null', b => b)
```

## Assignations & State Modification
As a general rule you should avoid assignations at all costs, they alter state and increase the risk of sharing state in code and changing functions parameters.
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

## Avoid If Expressions
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

## Prefer Ternary Expression Over || and &&
Prefer the usage of ternary expression where you are explicit of the else condition.
``` javascript
// ok
(foo) => foo && bar || 0

// good
(foo) => foo ? bar : { baz: true }
(foo) => foo ? baz : 0
```

## Use Ternary Expression to Determine data/functions Rather Than Execution
``` javascript
// bad
const foo = data => make => make2 => contition => contition ? make(data) : make2(data)

// good
const foo = make => make2 => contition => contition ? make : make2
foo(make)(make2)(contdition)(data)
```

## Avoid Curly Braces for Code Blocks
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

## Keep Variables Close to Usage
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

## Regex
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

## Function Type Signature Documentation (Hindley-Milner)
You should document all functions with Hindley-Milner annotation, it is the prevalent type signature documentation in functional languages (Haskell, Elm, OCaml, etc.) and it just makes a lot of sense to use it in functional Javascript.
```
// functionName :: type -> type -> type

// add :: Number -> Number -> Number
const add = x => y => x + y

// concat :: Array -> Array -> Array
const concat = a => b => a.concat(b)

// filter :: (Any -> Boolean) -> Array -> Array
const filter = f => a => a.filter(f)

// map :: (Any -> Any) -> Array -> Array
const map = f => a => a.map(f)

// replace :: Array -> String -> String
const replace = ([subStr, newSubstr]) => s => s.replace(subStr, newSubstr)

// uncurriedMaxOfThree :: (Number, Number, Number) -> Number
uncurriedMaxOfThree (x, y, z) = Math.max(x, y, z)

// curry :: ((Any, Any) -> Any) -> Any -> Any -> Any
const curry = f => x => y => f(x, y)

// uncurry :: (Any -> Any -> Any) -> (Any, Any) -> Any
const uncurry => f => (x, y) =>  f(x)(y)
```

### Parentheses in Hindely-Milner
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
// The first argument is expected to be of type (Any -> Any)
// so it would be incorrect to omit the parentheses.

// apply :: (Any -> Any) -> Any -> Any
const apply = f => x => f(x)
```


Finally, while this final case is optional, parentheses can be used to encourage the user to partially apply a function. Indeed some functions such as `curry` are rarely, if ever, fully evaluated.

```
// Both signatures are correct, but the second one indicates to the user
// that curry should be partially applied to return a function to be reused later

// curry :: ((Any, Any) -> Any) -> Any -> Any -> Any
// curry :: ((Any, Any) -> Any) -> (Any -> Any -> Any)
const curry = f => x => y => f(x, y)

// The end parentheses are optional since Any -> Any -> Any is equivalent to (Any -> Any -> Any)
```

## Partial Evaluation

### A Word On Partial Application

Partial application is when you give some of its arguments to a function to return a function that takes fewer arguments. The process of making a function partially applicable is also referred as currying.

```
// add :: Number -> Number -> Number
const add = x => y => x + y

// addTwo is a partial application of add

// addTwo :: Number -> Number
const addTwo = add(2)

addTwo(5) //=> 7
```

### More Than Just Application

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
// filterArraysThenConcat :: (Any -> Boolean) -> Array -> Array -> Array
const filterArraysThenConcat = myFilter => firstArray => {
  const filteredFirstArray = firstArray.filter(myFilter)
  return secondArray => filteredFirstArray.concat(secondArray.filter(myFilter))
}

const myFavouriteNumbers = [1, 2, 3, ..., 999999, 1000000]
const evenFilter = n => n % 2 === 0

// extendMyArrayOfEvenNumbers :: Array -> Array
const extendMyArrayOfEvenNumbers = filterArraysThenConcat(evenFilter)(myFavouriteNumbers)

// This call is way more efficient since the first filter has already been applied
extendMyArrayOfEvenNumbers([-1, -2, -3])
```

### Usage

Partial evaluation is a useful optimization tool when used properly. Its main downside is readability, observe that we had to break our [rule about curly braces](https://github.com/aldo-dev/javascript#avoid-curly-braces-for-code-blocks) in the process. For that reason it should not be systematically used.

#### When To Use

- Partially applied functions that you intend to reuse a lot
- Partially applied functions which execution can be arbitrarily long (ex: operations on huge Arrays)

#### When Not To Use

- When readability becomes an issue, maybe write smaller composable functions
- Never use partial evaluation for impure functions
- Whenever it is not absolutely necessary!

## Constants
You should just use magic number and string literals. There is no point in confusing the reader with data are hidden behind variables names, that are likely inappropriate.
``` javascript
// bad
const USER_DEFAULT_COUNT = 5
...
const reducer = (state = USER_DEFAULT_COUNT) => ... // you just can't know that the default is 5, it makes no sense

// good
const reducer = (state = 5) => ...
```

## Object Assign
Use `Object.assign` to avoid mutating state.
``` javascript
// bad
const state => {
  state.id = 1
  return state
}

// good
const state => Object.assign({}, state, { id : 1 }) // native js

const state => assign(state, { id : 1 }) // oncha api

const state => ({ ...state, id : 1 }) // es6 syntax
```

## Let, Const, Var
There can be just one: `const`. you should never use `var` and avoid at all cost `let`.

## Pattern Matching
Although pattern matching or destructuring is a cool tool in es6, it binds the function to the json structure.
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

## For, Loops, While, Foreach

Loops are inherently imperative. They also mix concerns: iteration and execution are two different concerns that, if handled separately from each other, result in more flexible code.

Better options include:

* [recursion](#recursion--tail-call)
* map
* filter
* reduce

## Recursion & Tail Call
To make a tail call recursion you need to place the function call at the end of your function and have it return the value. [see es6-recursion-tail-recursion](http://www.door3.com/insights/es6-recursion-tail-recursion)
``` javascript
// recur :: Number -> Number -> Number
const recur = n => acc =>  n == 0 ? acc : recur(n-1)(n * acc)

// recur :: Number -> Number
const factorial = (n) => recur(n)(1)
```

## New Keyword
Never use the new keyword.

## This Keyword
Because we code pure function and avoid shared state we must not use the `this` keyword, ever.

## Function Keyword
You should favor arrow functions over the function keyword.

## Arrow Functions : is the default, noop, identity, always
You should always use array function if you can. It's shorter more concise lambda and does not come with an implicit state but it's lexical context.

## Always
```javascript
// bad
function () { return 'Hello World' }

// good
() => 'Hello World'
```

## Identity
```javascript
// bad
function (a) { return a }

// good
a => a
```

## Noop
```javascript
// bad
function () { }

// good
() => { }
```
