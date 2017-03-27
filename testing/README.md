# Aldo Test Guide
When building javascript applications at Aldo there are 2 types of tests we promote, Functional Tests and Unit Tests through TDD. They are an essential part of our quality strategy. With Unit Tests, we test the business logic by writing tests against the functional components. While we test the system boundaries and the user interactions via Functional Tests.

# Functional Tests (FT)
For functional tests, we use _ROBOT Framework_ with _Selenium WebDriver_.

## Tests Organization
Test files should be in the `e2e` folder at the root of the repository.

```
.
+-- e2e
|   +-- robot
|       +-- tests
|           +-- epics
|               +-- (epic link)
|                   +-- (task id)
```

# Test Driven Development (TDD)
TDD is a developer discipline, where you write the test before the implementation. it's fundamental in the effort to deliver a better quality of software because it [reduces production bug density 40% — 80%](https://www.computer.org/csdl/mags/so/2007/03/s3024.pdf).

## 3 laws of TDD
1. You are not allowed to write any production code unless it is to make a failing unit test pass.
2. You are not allowed to write any more of a unit test than is sufficient to fail; and compilation failures are failures.
3. You are not allowed to write any more production code than is sufficient to pass the one failing unit test.
    > ![Uncle Bob 3 rules of TDD](http://butunclebob.com/ArticleS.UncleBob.TheThreeRulesOfTdd)

# Unit Tests
- We do it at the module level.
- A module must be pure.
    - Test pure composition of pure functions.
- We will not mock boundaries and neither tests them.
- If a composition needs data from a boundary, then we pass the sample of data to the composition and test the result.
- A test should be:
    * Simple.
    * Easy to maintain.
    * Easy to understand.

## Tools
Because we are testing only pure implementations the test requirements are quite simple. There is no need for mocking of spying and the only tool that is really needed for unit tests is a test runner such a _mocha_ or _Tape_. We use nodejs native library for asserts.

## Tests Organization
Test files should be in a `tests` folder near implementation.

There is less noise when scanning the implementation files and it's easier to locate them.
> e.g. Modules in `src/products/` should have their test files in `src/products/tests/`

> Q. Why do we suffix .test.js when already place that file in a tests sub folder ?

> A. Because it's easier to locate tests vs it's implementation

```
// bad
.
+-- src
|   +-- shoes
|       +-- high-heel.js
|       +-- snickers.js
|       +-- high-heel.test.js
|       +-- snickers.test.js

// good: single file
.
+-- src
|   +-- shoes
|       +-- high-heel.js
|       +-- tests
|           +-- high-heel.test.js

// good: multiple files
.
+-- src
|   +-- shoes
|       +-- high-heel.js
|       +-- snickers.js
|       +-- tests
|           +-- high-heel.test.js
|           +-- snickers.js
```