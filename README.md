# javascript
All the details about writing javascript @ Aldo

# import / require / export
## Motivation
Require is a library while import for System.import in the browser and import in nodejs are native implmentations.

## require
Don't use it, see [#Import]

## import
Imports should be done with the import statement and require should be avoided.
```
import potato from 'potato'
import {c arrot } from 'vegetables'
```
In the case of code that is ran without transpilation `require` can be used exceptionally, but this practice is discouraged because you can always used `node-babel`.

Imports should favor the absolute style
DO:
```
import potato from 'vegetables/potato'
```
DO NOT:
```
import potato from '../vegetables/potato'
```
note that when you are outside of webpack this does not work, so it is best practice to use webpack. It it is impossible you could use relative paths.

- export

# Subjects
- default export
- if, else
- tenary expression
- dangling comma
- semi-column
- Pull Request process
- pull request approvers
- curly braces
- who can merge
- documenting components owners, etc
- function signature documentation (Hindley-Milner)
- definition of done
- function purity
- TDD
- unit tests
- programming against an interface
- user test automation
- react components design and architecture
- monads
- composition
- constants
- DRY
- reusability
- copy code first
- the code will fork
- coupling of modules
- module pattern
- imputity
- shared state mutation
- local state mutation
- linter
- object.assign 
- utils
- helpers
- third-parties
- continuous integration
- devops
- acceptance tests
- imperative functions
- closer to usage
- business concerns separations
- higher order functions
- first class functions
- first class object
- learning resources
- react specific syntax
- let, const, var
- null undefined
- pattern matching
- tail call recurtions
- for, loops, while, foreach
- recurtions
- map, reduce
- Types: String, Number, etc
- Generators
- Types: Maybe, Identity, etc.
- closures
- curry and partial application
- one input, one output
- boy scout principle
- design patterns
- regex
- new keyword
- test tools
- mocking
- this
- unary operator
- function keyword
- arrow functions
- return keyword
- control flow
- single return by function
- ramda, underscore, jQuery, lodash
- configuration
- secret variables
- env variables
- assignations
- await async
- tupple
- http
- promises
- ||, &&, ===, !==, etc
- dependecy injections
- SOLID
- todo, task in code vs task software
