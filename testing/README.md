![it's a  draft](https://cdn.meme.am/cache/instances/folder781/66247781.jpg)

# Aldo Test Guide
All the details about writing tests @ Aldo

TDD

Integration test


# Test & TDD
It's a strange thing to write about TDD so late in a guide, but it is still most important.

## TDD: is the only way to do it
- It's a discpline, it should be done incrementally.
- 3 laws of TDD
- Benefit, fun facts about TDD (20-80% reduction of bugs density)
## unit tests: at the module level
- Test pure composition from a pure module.
- We don't test / mock boundaries.
- Pass input json -> composition. Test response
## programming against an interface (function) : first do it, then code impl
- Tests should be in a folder near implementation. There is less noise when scanning the implementation files and it's easier to locate tests, e.g. src/products/, src/products/tests
## user test automation: for each accepatance test

## test tools : don't, on ROBOT? investigation needed
## mocking : don't
- We should do mocking. It add useless complexity. Instead pure module should be tested.


# devops

## continuous integration
## devops
## acceptance tests
