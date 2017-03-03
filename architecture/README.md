![it's a  draft](https://cdn.meme.am/cache/instances/folder781/66247781.jpg)

# Aldo Javascript Architecture Guide
All the details about writing javascript @ Aldo

# airbnb
Whatever is not covered in this guide you should look at [aribnb/javascript](https://github.com/airbnb/javascript). Moreover the linter configuration speaks for itself.

# Values
- Modular architecture comes first
- Separation of concerns based on business requirements
- Small is better
- Less is more


## Separation of concerns based on business requirements
Avoid at all cost technical modules. They should be centic to a business concern and separated as such.
``` javascript
// bad
_ src
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
|   └── apple.component.js
|   └── apple.container.js
|   └── apple.saga.js
├── microsoft
|   └── microsoft.component.js
|   └── microsoft.container.js
|   └── microsoft.saga.js
└── index.html
```




# Code Organisation

## Utils module and utils folders
Do not use utils folder there you put non business logic code. This couples the codebase uselessly and increases its own complexity. Rather promote utils to a npm repository. If the usage is common enough to be on npm it could already be there or you will serve the nodejs open source community.

## Helpers and utils with business logic
Do not use the helper naming and concept. A function has only one purpose and its name should represent that, moreover it should be private to its module. Although it may cause code duplication, it will decrease complexity by decoupling modules.




# externals
## third-parties : must pass quorum approval process
## ramda, underscore, jQuery, lodash : yeah you need to stop using those


## copy code first
## the code will fork
## coupling of modules
## module pattern


# OOP principles
## SOLID
## DRY
## reusability
## design patterns



# Other Topics
## configuration
## secret variables
## env variables