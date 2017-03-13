![it's a  draft](https://cdn.meme.am/cache/instances/folder781/66247781.jpg)

# Aldo Test Guide
All the details about writing tests @ Aldo

Integration test

# Test & TDD
It's a strange thing to write about TDD so late in a guide, but it is still most important. TDD is not about practicing or increasing code coverage.

A test suite should be considered as documentation artefact. It should be explain what a feature does.

It should be easy to identify where a failure occur.

## TDD: is the only way to do it. Use common sense
- It's a discpline, it should be done incrementally.
- Pretending coverage is high is _NOT_ TDD
- 3 laws of TDD from uncle bob are:

1. You are not allowed to write any production code unless it is to make a failing unit test pass.
2. You are not allowed to write any more of a unit test than is sufficient to fail; and compilation failures are failures.
3. You are not allowed to write any more production code than is sufficient to pass the one failing unit test.
    > ![Uncle Bob 3 rules of TDD](http://butunclebob.com/ArticleS.UncleBob.TheThreeRulesOfTdd)


## Unit tests: 
- We do it @ the module level
- A module must be pure
    - Test pure composition of pure functions.
- We will not mock boundaries and neither test them.
- If a composition need data from a boundary, then we pass sample of data to the composition and test the result.
- A test should be:
    * Simple
    * Easy to maintain
    * Easy to understand

## programming against an interface (function) : first do it, then code impl
- Test files should be in a folder near implementation.

There is less noise when scanning the implementation files and it's easier to locate them.
> e.g. Modules in `src/products/` should have their test files in `src/products/tests/`

## Refactor the actual implementation flow
- Do refactor incrementally to pure function
- Run the existing test suite to make sure the system behave like the existing implementation
- Refactor the tests so that their are *Simple*, *Easy to maintain* and *Easy to understand*
- And yeah, just test the pure composition
- Run the test
- Create a PR

## user test automation: for each accepatance test

## test tools : don't, on ROBOT? investigation needed

Sinon, chai, mockery, etc are great tools. But they are all coming after what's not native to nodeJS.

- NodeJS can already do assert, let's use it.
- NodeJS can't do mocking. Huh oh, we don't do mocking, we test pure composition.

## Mocking
### Don't
- Mock
- It add useless complexity.
### Do
- Test pure module composition.


# devops

## continuous integration
## devops
## acceptance tests
