# Aldo Javascript Functional Programming Guide
All the details about writing javascript @ Aldo

# airbnb
Whatever is not covered in this guide you should look at [aribnb/javascript](https://github.com/airbnb/javascript). Moreover the linter configration speaks for itself.

# Motivation
The javascript functional programming guide was created to scope what can be done with javascript to promote a functional code base. At Aldo we believe that high level programming language like javascript benefit more from the maintainability of the functional style than a imperative performance focus style.

This guide aims not to restrict the style of individual contributors but set common ground for solving problems as a team with the Javascript language.

This guide is opinionated, it is not the *ideal* or *better* way of programming in Javascript nor programming Functional Javascript, it is a idea of how it may be done. Anyone may use it, we at Aldo feel it's the better way to do it.

You may use it, but at your own risks. We open source this guide in hopes it heals other derive work from it or simply use it and maybe help us improve it.

# Values
These are the values we uphold with this guide, and hope that this will help at releasing better software more regularly and steadily.

- Functional style prevails
- Imperative is not permited
- Pure functional programming
- Use Monads for flow control
- Everything must be composable
- Separation of concerns based on business requirements
- Modular architecture comes first
- Impurity must be rejected
- Small is better
- Less is more

# Learning Functional Javascript
There are a lot of functional programming resources that can help you understand better the benefits of functional programming, and of these in javascript, you ought to acquire that knowledge first.

## [Mostly Adequate Guide](https://drboolean.gitbooks.io/mostly-adequate-guide)
`Brian lonsdrof get to the point with his Mostly Adequate Guide, although it's a littelbit dated and was written before es6 this is still a gold mine.`

## [Professor Frisby Introduces Composable Functional JavaScript](https://egghead.io/lessons/javascript-linear-data-flow-with-container-style-types-box)
`Professor Frisby teaches composable programing in this serie, it is the softest introduction for Monads in javascript there is.`

# Functional Programming the good, the bad, and what you can do.
## control flow
You should use monads for control flow
``` javascript
// bad
const makeFoo = foo => {
  if (foo) {
  throw new Error('foo cannot be null')
  }
  return make(foo)
}

// good
const makeFoo =
  foo =>
    Either.fromNullable(foo)
      .fold(make)
```
*more examples meeded*

## function purity: is paramount
You should compose impurity instean of closing onto them.
``` javascript
import { readFileSync } form 'fs'
export default const readFileBody = filePath => {
  const fileContents = readFileSync(url)
  const json = JSON.parse(fileContents)
  return json.body
}

// good
import { readFileSync } form 'fs'
import compose from 'oncha/compose'
const getBody = document => document.body
export default const readFileBody = compose(getBody, JSON.parse, readFileSync)
```
## Single return by function
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

## one input, one output
``` javascript
// bad
const add (a, b) => a + b

const foo (a, b, c, d, e) => (/* ... */)
const foo = a => b => c => d => e => (/* ... */)

// good
const add ({a, b}) => a + b
const add ([a, b]) => a + b

// best
const add a => b => a + b
```

## Split code into composable function
``` javascript
// bad
const headerStringToObject = headerString => {
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
const prepare = compose(fromPairs, map(split('=')), map(trim), split(','), toString)
const headerStringToObject = compose(combine, prepare)
```

## Do not program imperative functions
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
const fromPair = ([key, value]) => ({ [key]: value })
const fromPairs = map(fromPair)
```

## Do not use null and undefined for flow control
FLow control shoud be done through monads.
``` javascript
// bad
const getBody = document => document && document.body ? document.body : undefined
const getBody = document => document && document.body && document.body

// good, a monadic api is more reliable and differs the decisions to the caller
const getBody = document =>
  Either.formnNllable(document)
  .map(d => d.body)

// good
const getBody = document =>
  Either.formnNllable(document)
  .map(d => d.body)
  .fork(() => 'can\'t get body of null', b => b)
```

## assignations & state modification
As a general roule you should avoid assignations at all costs, they alter state and increase the risk of sharing state in code and changin fucntions parameters.
``` javascript
// bad
const foo = bar => bar = bar || 0

// good
const foo = bar => bar ? bar : 0
```
*could use better examples*
- shared state mutation : disallowed
- local state mutation : disallowed




- first class functions : is the standard tool for composition
- higher order functions: : is the standard tool for composition
- curry and partial application : is the standard tool for composition


- impurity : impure function can only be composed, never injected or closed uppon. Thye must be tagged with `//IMPURE`
- tail call recurtions : you shoul use it as a default tool for recursion
- map, reduce : for standard data manipulation
- Types: String, Number, etc : know your types
- Generators : how to use them
- Types: Maybe, Identity, etc.


# Code Organisation
- utils
- helpers
- copy code first
- the code will fork
- coupling of modules
- module pattern
- business concerns separations

# style
- if, else
- tenary expression
- dangling comma
- semi-column
- curly braces
- function signature documentation (Hindley-Milner)
- constants
- object.assign
- linter
- closer to usage
- let, const, var
- pattern matching
- for, loops, while, foreach
- recurtions
- regex
- new keyword
- Generators : syntax
- this : no, don't use it
- unary operator : over tenary operations
- function keyword : you should not be using it
- arrow functions : is the default, noop, identity, always
- return keyword : disallowed
- await async : use a future
- tupple : least as possible
- promises : use a future
- ||, &&, ===, !==, etc
- dependecy injections

# Test & TDD
It's astrange thing to write about TDD so late in a guide, but it is still most important.

- TDD: is the only way to do it
- unit tests: at the module level
- programming against an interface : first do it, then code impl
- user test automation: for each accepatance test
- test tools : don't, on ROBOT? investigation needed
- mocking : don't

# process
- Pull Request process
- pull request approvers
- who can merge
- documenting components owners, etc
- boy scout principle
- todo, task in code vs task software
- definition of done


# React
- react components design and architecture
- css
- react specific syntax


# externals
- third-parties : must pass quorum approval process
- ramda, underscore, jQuery, lodash : yeah you need to stop using those

# devops
- continuous integration
- devops
- acceptance tests

# OOP principles
- SOLID
- DRY
- reusability
- design patterns

# Other Topics
- configuration
- secret variables
- env variables
- http

# Topics that are covered by airbnb
- module export / import / require

# Contribution and changes to the guide
To make a change to this guide you must create a pull request with your changes. The change must be debated in the pull request by anyone, but must uphold the [#Values] and must have the unanimous yes of the guide quorum.

# Aldo Javascript Functional Programming Guide Quorum
Simon Deshaies
Jean-Francois Dube
Ysael Pepin