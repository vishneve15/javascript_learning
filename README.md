# Node.js Style Guide() {

## Table of Contents

  1. [Use-Strict](#use-strict)
  1. [Objects](#objects)
  1. [Arrays](#arrays)
  1. [Strings](#strings)
  1. [Functions](#functions)
  1. [Properties](#properties)
  1. [Variables](#variables)
  1. [Requires](#requires)
  1. [Promises](#promises)
  1. [Callbacks](#callbacks)
  1. [Try-catch](#try-catch)
  1. [Hoisting](#hoisting)
  1. [Conditional Expressions & Equality](#conditional-expressions--equality)
  1. [Comments](#comments)
  1. [Whitespace](#whitespace)
  1. [Commas](#commas)
  1. [Semicolons](#semicolons)
  1. [Naming Conventions](#naming-conventions)
  1. [Accessors](#accessors)
  1. [Constructors](#constructors)
  1. [Folder Structure](#folder-structure)
  1. [Custom Modules](#custom-modules)

## General Rules
  - Use your brain before using your keyboard.
  - No unreachable code after return, throw, continue, and break statements.
  - No unused variables or functions.
  - No multiple new lines.
  - There are no general rules. But if you are planning to ignore one, you'd better have a good reason for it. 

## Use Strict

  - Add the sentence `'use strict'` on top of your code.
  [Reference](https://www.w3schools.com/js/js_strict.asp)

**[⬆ back to top](#table-of-contents)**

## Objects

  - Use the literal syntax for object creation.

    ```javascript
    // bad
    let item = new Object()

    // good
    let item = {}
    ```

  - Use readable synonyms in place of reserved words.

    ```javascript
    // bad
    let superman = {
      class: 'alien'
    }

    // bad
    let superman = {
      klass: 'alien'
    }

    // good
    let superman = {
      type: 'alien'
    }
    ```

   - Put short declarations on a single line. 
   
    ```javascript
    //bad
    var a = [
    'hello', 'world'
    ]

    //good
    let a = ['hello', 'world']
    ```

   - Only quote keys when your interpreter complains

    ```javascript
    //bad
    let obj = {
        'normalProperty' : 'value',
        'weird-property': 'value',
        'even weirder': 'value'
    }
    
    //good
    let obj = {
        normalProperty : 'value',
        'weird-property': 'value',
        'even weirder': 'value'
    }
    ```

 - Checking if an object has elements
    ```javascript
    let obj = {a:1}
    //bad
    if(obj.length) console.log(obj) //obj.length == undefined

    //good
    if(Object.entries(obj).length) console.log(obj) //evaluates to true. see section [Conditional Expressions & Equality]
    ```

  - Destructuring an object as variables (use case: getting value from config file)
    ```javascript
    let obj = {
        a: 1,
        b: 2,
        c: 3
    }

    //bad
    let a = obj.a
    let b = obj.b
    let c = obj.c

    //good
    let {a, b, c} = obj

    console.log(`${a} ${b} ${c}`) // logs '1 2 3'
    ```

  - Even consider this: destructuring directly as parameter. (DEPENDS ON EACH CASE)
    ```javascript
    let obj = {
        firstName : 'Nacho',
        lastName : 'Bibián'
    }

    // good
    let getFullName = (user) => {
       const {firstName, lastName} = user
       return `${firstName} ${lastName}`
    }

    // best
    let getFullName = ({ firstName, lastName }) => {
       return `${firstName} ${lastName}`
    }

    //run
    getFullName(obj)
    ```

  - Merging objects using spread operator
    ```javascript
    let a = {prop1: 1, prop2: 2}
    let b = {prop3: 3, prop4: 4}
    let c = {...a, ...b}
    ```

  - IMPORTANT! Cloning Objects
    ```javascript
    //bad
    let copy = original 
    ```
    This makes a reference to the object, not a copy. So making changes in `copy` affects `original`.

    ```javascript
    // if shallow copy is enough we use the spread operator.
    let copy = {...original}

    // if we need deep copying (original has objects as properties)
    const _ = require('lodash')
    let copy = _.cloneDeep(original)

    ```
    Shallow copy will clone the properties on the first level, but reference the rest levels.

  - To iterate through the properties of an Object we'll use a for ... in loop.
  ```javascript
    let object = {a:1,b:2,c:3}
    for (let p in object){
      console.log(`Value of ${p} is ${object[p]}`)
    }
  ```
  https://hackernoon.com/5-techniques-to-iterate-over-javascript-object-entries-and-their-performance-6602dcb708a8


**[⬆ back to top](#table-of-contents)**

## Arrays

  - Use the literal syntax for array creation

    ```javascript
    // bad
    let items = new Array()

    // good
    let items = []
    ```

  - Checking if element is an Array (before operating with it)
    ```javascript
    //good
    Array.isArray(element) //returns boolean
    ```

  - Iterating through an array: if we want a final array as a result then map, otherwise forEach.
    ```javascript
    let array = [1,2,3]
    
    //bad
    array.map(element => console.log(element*element))

    //good
    array.forEach(element => console.log(element*element))

    //bad
    let result []
    array.forEach(element => result.push(element*element))

    //good
    let result = array.map(element => {return element*element})
    ```
  
  - Make use of the different array operators (filter(),reduce(),...spread)
    ```javascript
    let noEmpties = array.filter(x => x)
    let avg = array.reduce((prev,cur) => prev+cur, 0) / array.length
    let arr1 = [0, 1, 2]
    let arr2 = [3, 4, 5]
    arr1.push(...arr2)
    ```

  - If performance is VERY critical, use for loops instead of .forEach

  - CAREFUL! Cloning an array is not as simple as with other types
    ```javascript
    let string = 'hello'
    let string2 = string
    string2 += ' friend'
    console.log(string) // returns 'hello'
    console.log(string2) // returns 'hello friend'
     
    let array = [1,2,3]
    let array2 = array
    array2.pop()
    console.log(array) // returns [1,2]
    console.log(array2) // returns [1,2]
    ```
  - Don’t change the properties of an array like a regular object. Always rely on the methods provided to operate on its elements.
  - When you need to copy an array use .slice() [jsPerf](http://jsperf.com/converting-arguments-to-an-array/7)

    ```javascript
    let len = items.length
    let itemsCopy = []

    // bad
    let i
    for (i = 0; i < len; i++) {
      itemsCopy[i] = items[i]
    }

    // good
    itemsCopy = items.slice()
    ```

**[⬆ back to top](#table-of-contents)**


## Strings

  - Use single quotes `''` for strings

    ```javascript
    // bad
    let name = "Bob Parr"

    // good
    let name = 'Bob Parr'

    // bad
    let fullName = "Bob " + this.lastName

    // good
    let fullName = 'Bob ' + this.lastName
    ```

  - If we have variables INSIDE our string, use `` instead

    ```javascript
    //bad 
    let message = 'Hello ' + name + ', how are you?'

    //good
    let message = `Hello ${name}, how are you?`
    ```

  - Double quotes are reserved for JSON objects

  - Strings longer than 80 characters should be written across multiple lines using string concatenation.
  - Note: If overused, long strings with concatenation could impact performance. [jsPerf](http://jsperf.com/ya-string-concat) & [Discussion](https://github.com/airbnb/javascript/issues/40)

    ```javascript
    // bad
    let errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.'

    // bad
    let errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.'

    // good
    let errorMessage = 'This is a super long error that was thrown because ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.'
    ```

**[⬆ back to top](#table-of-contents)**


## Functions

  - Use always arrow functions. MIND THE SPACES

    ```javascript
    // named function expression
    let named = () => {
      return true
    }
    ```

  - Constructors can't be created with arrow functions [See section](#constructors)

  - Avoid immediately-invoked function expressions (IIFE). Instead name and run
    ```javascript
    //bad
    (() => {
      console.log('Welcome to the Internet. Please follow me.')
    })()

    //good
    let welcome = () => {
      console.log('Welcome to the Internet. Please follow me.')
    }
    welcome()
    ```

  - Never declare a function in a non-function block (if, while, etc). Assign the function to a variable instead.

    ```javascript
    // bad
    if (currentUser) {
       test() =>  {
        console.log('Nope.')
      }
    }

    // good
    var test
    if (currentUser) {
      test = () => {
        console.log('Yup.')
      }
    }
    ```

  - Never name a parameter `arguments`, this will take precedence over the `arguments` object that is given to every function scope.

    ```javascript
    // bad
    let nope = (name, options, arguments) => {
      // ...stuff...
    }

    // good
    let yup = (name, options, args) => {
      // ...stuff...
    }
    ```

  - Never reassign or mutate parameters
    ```javascript
    //bad
    let doSome = (a) => {
        a = 'value'
    }

    //also bad
    let doSome = (a) => {
        a.property = 'value'
    }

    //good
    let doSome = (a) => {
        let b = a
        b.property = 'value'
    }
    ```

  - Use default parameters when appliable
    ```javascript
    let read = (obj = 'text') => {
        console.log(obj)
    }
    read('something') // logs 'something'
    read(null) // logs 'text'
    read() // also logs 'text'
    ```

  - Place default parameters last (better readability)
    ```javascript
    //bad
    let read = (obj = 'text', user) => {
        sendTo(user,obj)
    }

    //good
    let read = (user, obj = 'text') => {
        sendTo(user,obj)
    }
    ```

**[⬆ back to top](#table-of-contents)**


## Properties

  - Use dot notation when accessing properties.

    ```javascript
    let luke = {
      jedi: true,
      age: 28
    }

    // bad
    let isJedi = luke['jedi']

    // good
    let isJedi = luke.jedi
    ```

  - Use subscript notation `[]` when accessing properties with a variable or when the key has special symbols (preferrably that shouldn't happen).

    ```javascript
    let luke = {
      jedi: true,
      age: 28
    }

    let getProp = (prop) => {
      return luke[prop]
    }

    let isJedi = getProp('jedi')
    ```

**[⬆ back to top](#table-of-contents)**


## Variables

  - Always use `const` or `let` to declare variables. Not doing so will result in global variables. We want to avoid polluting the global namespace. Captain Planet warned us of that.

    ```javascript
    // bad
    superPower = new SuperPower()

    // good
    let superPower = new SuperPower()

    //good
    const fs = require('fs')
    ```

  - Declare each variable on a newline, with a `let` before each of them.

    ```javascript
    // bad
     let items = getItems(),
          goSportsTeam = true,
          dragonball = 'z'

    // good
     let items = getItems()
     let goSportsTeam = true
     let dragonball = 'z'
    ```

  - Declare unassigned variables last. This is helpful when later on you might need to assign a variable depending on one of the previous assigned variables.

    ```javascript
    // bad
    let i
    let items = getItems()
    let dragonball
    let goSportsTeam = true
    let len

    // good
    let items = getItems()
    let goSportsTeam = true
    let dragonball
    let length
    let i
    ```

  - Avoid redundant variable names, use `Object` instead.

    ```javascript
    // bad
    let kaleidoscopeName = '..'
    let kaleidoscopeLens = []
    let kaleidoscopeColors = []

    // good
    let kaleidoscope = {
      name: '..',
      lens: [],
      colors: []
    }
    ```

## Requires

  - Use `const`
  - Organize your node requires in the following order:
      - core modules
      - npm modules
      - others

    ```javascript
    // bad
    const Car = require('./models/Car')
    const async = require('async')
    const http = require('http')

    // good
    const http = require('http')
    const fs = require('fs')

    const async = require('async')
    const mongoose = require('mongoose')

    const Car = require('./models/Car')
    ```

  - Do not use the `.js` when requiring modules

    ```javascript
    // bad
    var Batmobil = require('./models/Car.js')

    // good
    const Batmobil = require('./models/Car')

    ```

**[⬆ back to top](#table-of-contents)**

## Promises

 - Use Promises. Note the difference between `resolve(false)` and `reject(error)`

    ```javascript
    //good
    let getName = (obj) => {
        return new Promise((resolve,reject) => {
            try{
                let result = fetchName()
                if(result) resolve(true)
                else resolve(false)
            }
            catch(error) reject(error)
        })
    }
    ```

 - Use and chain promises

    ```javascript
    //good
    getName(obj)
    .then(result => {return getCity(result)})
    .then(result => {return getCountry(result)})
    .then(result => console.log(result))
    .catch(e => handleError(e))
    ```

**[⬆ back to top](#table-of-contents)**

## Callbacks

  - In general, use Promises instead **[go to promises](#promises)**

  - Always check for errors in callbacks

    ```javascript
    //bad
    database.get('pokemons', (err, pokemons) => {
        console.log(pokemons)
    })

    //good
    database.get('dragonballs', (err, dragonballs) => {
        if (err) {
        // handle the error somehow, maybe return with a callback
        return console.log(err)
        }
        console.log(dragonballs)
    })
    ```

  - Return on callbacks. Do it as soon as you can

    ```javascript
    //bad
    database.get('dragonballs', (err, dragonballs) => {
        if (err) {
        // if not return here
        console.log(err)
        }
        // this line will be executed as well
        console.log(dragonballs)
    })

    //good
    database.get('dragonballs', (err, dragonballs) => {
        if (err) {
        // handle the error somehow, maybe return with a callback
        return console.log(err)
        }
        console.log(dragonballs)
    })
    ```

  - Use descriptive arguments in your callback when it is an "interface" for others. It makes your code readable.

    ```javascript
    // bad
    getAnimals = (done) => {
        Animal.get(done)
    }

    // good
    getAnimals = (done) => {
        Animal.get((err, animals) => {
        if(err) {
            return done(err)
        }

        return done(null, {
            dogs: animals.dogs,
            cats: animals.cats
        })
        })
    }
    ```

**[⬆ back to top](#table-of-contents)**


## Try catch

- Only throw in synchronous functions

  Try-catch blocks cannot be used to wrap async code. They will bubble up to the top, and bring
  down the entire process.

    ```javascript
    //bad
    readPackageJson = (callback) => {
        fs.readFile('package.json', (err, file) => {
        if (err) {
            throw err
        }
        ...
        })
    }
    //good
    readPackageJson = (callback) => {
        fs.readFile('package.json', (err, file) => {
        if (err) {
            return  callback(err)
        }
        ...
        })
    }
    ```

- Catch errors in sync calls

    ```javascript
    //bad
    let data = JSON.parse(jsonAsAString)

    //good
    let data
    try {
        data = JSON.parse(jsonAsAString)
    } catch (e) {
        //handle error - hopefully not with a console.log)
        console.log(e)
    }
    ```

  - Use try catch inside Promises and reject the error
    ```javascript
    let myPromise = (input) => {
        return new Promise((resolve,reject) => {
            try{
                /*
                doing whatever
                we will resolve()
                we may as well reject, if appropriate
                */
            }
            catch(e) {reject(e)}
        })
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Hoisting

  - For more information refer to [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting) by [Ben Cherry](http://www.adequatelygood.com/)

**[⬆ back to top](#table-of-contents)**


## Conditional Expressions & Equality

  - When appliable, use ternary conditions. (Do NOT nest ternary statements)

    ```javascript
    //bad
    let value
    if(condition){
        value = 'option A'
    }
    else {
        value = 'option B'
    }
    
    //good
    let value = condition ? 'option A' : 'option B'
    ```

  - No ternary operators when simpler alternatives exist
    ```javascript
    //bad
    let score = val ? val : 0

    //good
    let score = val || 0
    ```

  - Use `===` and `!==` over `==` and `!=`.
  - Conditional expressions are evaluated using coercion with the `ToBoolean` method and always follow these simple rules:

    + **Objects** evaluate to **true**
    + **Undefined** evaluates to **false**
    + **Null** evaluates to **false**
    + **Booleans** evaluate to **the value of the boolean**
    + **Numbers** evaluate to **false** if **+0, -0, or NaN**, otherwise **true**
    + **Strings** evaluate to **false** if an empty string `''`, otherwise **true**

    ```javascript
    if ([0]) {
      // true
      // An array is an object, objects evaluate to true
    }
    ```

  - ALWAYS use shortcuts when possible.

    ```javascript
    // bad
    if (name !== '') {
      // ...stuff...
    }

    // good
    if (name) {
      // ...stuff...
    }

    // bad
    if (collection.length > 0) {
      // ...stuff...
    }

    // good
    if (collection.length) {
      // ...stuff...
    }
    ```

  - Use equalities when returning booleans
    ```javascript
    //bad
    if(a === b) return true
    else return false

    //good
    return a===b

    //bad
    let a
    if(b === c) return true
    else return false    

    //good
    let a = b===c
    ```

  - Think about your conditions and clean them up
    ```javascript
    //bad 
    if(Number.isNumber(a) && b > a){}

    //good
    if(b > a) {} //if this is true, a is necessarily a number
    ```

  - Conditions are evaluated from left to right so use it when useful.
    ```javascript
    //Checking if an object has value and if its property has value

    //bad
    if(object.property){} //this throws an error if object is null or undefined

    //good
    if(object && object.property){} //if object is null or undefined second condition is skipped
    ```

  - Use descriptive conditions. 
  - Any non-trivial conditions should be assigned to a descriptively named variable or function

    ```javascript
    //bad
    if (password.length >= 4 && /^(?=.*\d).{4,}$/.test(password)) {
        console.log('losing')
    }

    //good
    var isValidPassword = password.length >= 4 && /^(?=.*\d).{4,}$/.test(password)

    if (isValidPassword) {
        console.log('winning')
    }
    ```

  - For more information see [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) by Angus Croll

**[⬆ back to top](#table-of-contents)**


## Comments

  - Use `/** ... */` for multiline comments. Include a description, specify types and values for all parameters and return values.

    ```javascript
    // bad
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param <String> tag
    // @return <Element> element
    let make = (tag) => {

      // ...stuff...

      return element
    }

    // good
    /**
     * make() returns a new element
     * based on the passed in tag name
     * @param <String> tag
     * @return <Element> element
     */
    let make = (tag) => {

      // ...stuff...
      return element
    }
    ```

  - Use `//` for single line comments.

    ```javascript
    // bad
    /* is current tab */
    let active = true

    // good
    // is current tab
    let active = true
    ```

  - Prefixing your comments with `FIXME` or `TODO` helps other developers quickly understand if you're pointing out a problem that needs to be revisited, or if you're suggesting a solution to the problem that needs to be implemented. These are different than regular comments because they are actionable. The actions are `FIXME -- need to figure this out` or `TODO -- need to implement`.

  - Use `// FIXME:` to annotate problems

    ```javascript
    let Calculator = () => {

      // FIXME: shouldn't use a global here
      total = 0

      return this
    }
    ```

  - Use `// TODO:` to annotate solutions to problems

    ```javascript
    let Calculator = () => {

      // TODO: total should be configurable by an options param
      this.total = 0

      return this
    }
    ```

**[⬆ back to top](#table-of-contents)**


## Whitespace

  - Use Visual Studio Code "Format Document" ALWAYS

  - Set off operators with spaces.

    ```javascript
    // bad
    let x=y+5

    // good
    let x = y + 5
    ```

  - End files with a single newline character.

    ```javascript
    // bad
    (global => {
      // ...stuff...
    })(this)
    ```

    ```javascript
    // bad
    (global => {
      // ...stuff...
    })(this)↵
    ↵
    ```

    ```javascript
    // good
    (global => {
      // ...stuff...
    })(this)↵
    ```

  - Use indentation when making long method chains.

    ```javascript
    // bad
    $('#items').find('.selected').highlight().end().find('.open').updateCount()

    // good
    $('#items')
      .find('.selected')
        .highlight()
        .end()
      .find('.open')
        .updateCount()

    // bad
    let leds = stage.selectAll('.led').data(data).enter().append('svg:svg').class('led', true)
        .attr('width',  (radius + margin) * 2).append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led)

    // good
    let leds = stage.selectAll('.led')
        .data(data)
      .enter().append('svg:svg')
        .class('led', true)
        .attr('width',  (radius + margin) * 2)
      .append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led)
    ```

**[⬆ back to top](#table-of-contents)**

## Commas

  - Leading commas: **Nope.**

    ```javascript
    // bad
    let hero = {
        firstName: 'Bob'
      , lastName: 'Parr'
      , heroName: 'Mr. Incredible'
      , superPower: 'strength'
    }

    // good
    let hero = {
      firstName: 'Bob',
      lastName: 'Parr',
      heroName: 'Mr. Incredible',
      superPower: 'strength'
    }
    ```

  - Additional trailing comma: **Nope.** This can cause problems with IE6/7 and IE9 if it's in quirksmode. Also, in some implementations of ES3 would add length to an array if it had an additional trailing comma. This was clarified in ES5 ([source](http://es5.github.io/#D)):

  > Edition 5 clarifies the fact that a trailing comma at the end of an ArrayInitialiser does not add to the length of the array. This is not a semantic change from Edition 3 but some implementations may have previously misinterpreted this.

    ```javascript
    // bad
    let hero = {
      firstName: 'Kevin',
      lastName: 'Flynn',
    }

    let heroes = [
      'Batman',
      'Superman',
    ]

    // good
    let hero = {
      firstName: 'Kevin',
      lastName: 'Flynn'
    }

    let heroes = [
      'Batman',
      'Superman'
    ]
    ```

**[⬆ back to top](#table-of-contents)**


## Semicolons

  - **Nope** (Unless someone convinces us)

    ```javascript
    // bad
    let lukeName = () => {
      let name = 'Skywalker';
      return name;
    })

    // good
    let lukeName = () => {
      let name = 'Skywalker'
      return name
    })

    ```

**[⬆ back to top](#table-of-contents)**


## Naming Conventions

  - Avoid single letter names. Be descriptive with your naming.

    ```javascript
    // bad
    let q = () => {
      // ...stuff...
    }

    // good
    let query = () => {
      // ..stuff..
    }
    ```

  - Use camelCase when naming objects, functions, and instances

    ```javascript
    // bad
    let OBJEcttsssss = {}
    let this_is_my_object = {}
    let c = () => {}
    let u = new user({
      name: 'Bob Parr'
    })

    // good
    let thisIsMyObject = {}
    let thisIsMyFunction = () => {}
    let user = new User({
      name: 'Bob Parr'
    })
    ```

  - Use PascalCase when naming constructors or classes

    ```javascript
    // bad
    let user = function(options){
      this.name = options.name
    }

    let bad = new user({
      name: 'nope'
    })

    // good
    let User = function(options){
      this.name = options.name
    }

    let good = new User({
      name: 'yup'
    })
    ```

  - Use a leading underscore `_` when naming private properties

    ```javascript
    // bad
    this.__firstName__ = 'Panda'
    this.firstName_ = 'Panda'

    // good
    this._firstName = 'Panda'
    ```

  - When saving a reference to `this` use `_this`.

    ```javascript
    // bad
    let aboutThis = () => {
      let self = this
      return () => {
        console.log(self)
      }
    }

    // bad
    let aboutThis = () => {
      let that = this
      return () => {
        console.log(that)
      }
    }

    // good
    let aboutThis = () => {
      let _this = this
      return () => {
        console.log(_this)
      }
    }
    ```


**[⬆ back to top](#table-of-contents)**


## Accessors

  - Accessor functions for properties are not required
  - If you do make accessor functions use getVal() and setVal('hello')

    ```javascript
    // bad
    dragon.age()

    // good
    dragon.getAge()

    // bad
    dragon.age(25)

    // good
    dragon.setAge(25)
    ```

  - If the property is a boolean, use isVal() or hasVal()

    ```javascript
    // bad
    if (!dragon.age()) {
      return false
    }

    // good
    if (!dragon.hasAge()) {
      return false
    }
    ```

  - It's okay to create get() and set() functions, but be consistent.

    ```javascript
    let Jedi = function(options){
      options || (options = {})
      let lightsaber = options.lightsaber || 'blue'
      this.set('lightsaber', lightsaber)
    }

    Jedi.prototype.set = (key, val) => {
      this[key] = val
    }

    Jedi.prototype.get = (key) => {
      return this[key]
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Constructors

  - Assign methods to the prototype object, instead of overwriting the prototype with a new object. Overwriting the prototype makes inheritance impossible: by resetting the prototype you'll overwrite the base!

    ```javascript
    function Jedi () {
      console.log('new jedi')
    }

    // bad
    Jedi.prototype = {
      fight: () => {
        console.log('fighting')
      },

      block: () => {
        console.log('blocking')
      }
    }

    // good
    Jedi.prototype.fight = () => {
      console.log('fighting')
    }

    Jedi.prototype.block = () => {
      console.log('blocking')
    }
    ```

  - Methods can return `this` to help with method chaining.

    ```javascript
    // bad
    Jedi.prototype.jump = () => {
      this.jumping = true
      return true
    }

    Jedi.prototype.setHeight = (height) => {
      this.height = height
    }

    let luke = new Jedi()
    luke.jump() // => true
    luke.setHeight(20) // => undefined

    // good
    Jedi.prototype.jump = () => {
      this.jumping = true
      return this
    }

    Jedi.prototype.setHeight = (height) => {
      this.height = height
      return this
    }

    let luke = new Jedi()

    luke.jump()
      .setHeight(20)
    ```

 - Notice that an arrow function cannot be used as a constructor. Let’s see what happens if however trying to:
   ```javascript
    let Message = (text) => {
    this.text = text;
    };
    // Throws "TypeError: Message is not a constructor"
    let helloMessage = new Message('Hello World!')
   ```

 - Executing new Message('Hello World!'), where Message is an arrow function, JavaScript throws a TypeError that Message cannot be used as a constructor. Instead: 

   ```javascript
    let Message = function(text) {
    this.text = text;
    };
   
    let helloMessage = new Message('Hello World!')
    console.log(helloMessage.text); // => 'Hello World!'
   ```

**[⬆ back to top](#table-of-contents)**

## Folder Structure

  - In general our folder structure will be the following
    ```javascript
    index.js
    package.json
    README.md
    Dockerfile
    .gitignore
    .dockerignore
    config
      -file.json
      -certificates
    middleware
      -routes.js
      -functions.js
      -controllers
        -index.js
      -models
        -index.js
    services
      -index.js
    test
      -samples
      -index.js
    ```

**[⬆ back to top](#table-of-contents)**

## Custom Modules
  - Use standard top level structure: `lib/`, `bin/`, `test/`, `test/integration/`, `examples/`, `src/`, `index.js`, `package.json`, `README.md`, `LICENSE`, `Changelog.md`
  - Use expressive non-creative name when possible
  - Use package.json scripts (at least start and test)
  - README:
     - use same examples you have in `test` or `examples` verbatim (prefer some tool that assemble them for you)
     - Cover at least everything visible from top require
  - Create small modules
  - Do it the unix-way:
    `
    Developers should build a program out of simple parts connected by well defined interfaces, so problems are local, and parts of the program can be replaced in future versions to support new features.
    `
  - Do not build Deathstars - keep it simple, a module should do one thing, but that thing well.
  - Start new projects with `npm init`.
  - Production/staging deployments should be done with environment variables. The most common way to do this is to set the NODE_ENV variable to either production or staging.

**[⬆ back to top](#table-of-contents)**

## The JavaScript Style Guide Guide

  - [Reference](https://github.com/airbnb/javascript/wiki/The-JavaScript-Style-Guide-Guide)

# }