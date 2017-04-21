# Architecture
A well architectured' javascript program is done through the module architectural pattern. Which decouple modules (components) and expose composable interfaces. Therefore it is paramount to properly apply the module pattern.

So what is a module? It is a self-contained program, that takes an input and return an output. This program can be pure or impure. But a pure program cannot receive an impure input. So expect inputs to be data. This program can have any umber of private function to achieve its goal. But as a general idea, it should have only one interface -- one export.

A module can require or import as many modules it needs to achieve its goal. But you should try to keep the module small and centered around one responsibility.

A module should be in its own file, and all of its sub-modules in the same directory. In this manner, a composed module can be replaced or deleted from the javascript program without any major side effects.

One could say that a complex javascript program is a collection of modules that could be replaced with a module with the same public interface.

# A Note About Reusability
Opposite to what some people may think, the idea of reusability is not so much a goal but a side effect in functional programming. It's something you attain because you design the components properly, not because you avoid copying every piece of code. We much rather see code, that will never change because it has only one responsibility, be copied over as a private function in another module than to be pushed as a utility or its own module. Moreover, think like this, copy the code first, use unit tests to document the functionality, globalization of the code is the last resort thing.

# Separation of concerns based on business requirements
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
├── microsoft
|   └── microsoft.action.js
|   └── microsoft.component.js
|   └── microsoft.container.js
|   └── microsoft.saga.js
└── index.html
```

# Utils module and utils folders
Do not use utils folder there you put the non business logic code. This couples the codebase uselessly and increases its own complexity. Rather promote utils to an npm repository. If the usage is common enough to be on npm it could already be there or you will serve the nodejs open source community.

# Helpers and utils with business logic
Do not use the helper naming and concept. A function has only one purpose and its name should represent that. Moreover, it should be private to its module. Although it may cause code duplication, it will decrease complexity by decoupling modules.
