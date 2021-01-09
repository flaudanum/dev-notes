<style>
  body {
    margin: auto;
    width: 50%;
    min-width: 800px;
  }
  h1, h2, h3 {
    color: rgb(29, 190, 137);
  }
</style>
<h1>Cypress</h1>

<h2> Table of Content </h2>

- [Cypress vs Selenium](#cypress-vs-selenium)
  - [Limitations](#limitations)
- [Dependencies](#dependencies)
- [Setup](#setup)
- [Cypress project](#cypress-project)
  - [Support](#support)
  - [Plugins](#plugins)
  - [Integration](#integration)
  - [Fixture](#fixture)
- [Selection of elements](#selection-of-elements)
  - [get vs. find](#get-vs-find)
- [Assertions](#assertions)
  - [BDD assertions](#bdd-assertions)
  - [TDD assertions](#tdd-assertions)
  - [Chai-jQuery](#chai-jquery)
  - [Sinon-Chai](#sinon-chai)
  - [Cypress <tt>should</tt> assertions](#cypress-ttshouldtt-assertions)
- [Debugging](#debugging)

## Cypress vs Selenium

Cypress is a fully integrated JS framework whereas Selenium is a cross-language library which must be assembled:

- programming language
- testing framework
- web driver

Cypress can mock APIs which Selenium cannot.

### Limitations

- Cypress does not support _MS IE_ and _Safari_ or earlier versions of _MS Edge_.
- No mobile testing.
- single tab and domain

You cannot use `async`/`await` because of Cypress' design (see the [official FAQ section](https://docs.cypress.io/faq/questions/using-cypress-faq.html#Can-I-use-the-new-ES7-async-await-syntax)).

## Dependencies

Cypress mostly relies on **Chai** and **JQuery**.

## Setup

Install _Cypress_ as a dev dependency in the project:

```
npm install --save-dev cypress
```

Start the Cypress runner:

```
> npx cypress open
It looks like this is your first time using Cypress: 5.3.0

  √  Verified Cypress! C:\Users\frederic.Laudarin\AppData\Local\Cypress\Cache\5.3.0\Cypress

Opening Cypress...
```

When the Cypress project does not exist 

`cypress.json` is at the root of the cypress directory. It use to configurate Cypress by overloading the default setting values.

```json
{
  "baseURL": "http://localhost:4200",
  "ignoreTestFiles": "**/examples/*",
  "viewportHeight": 1080,
  "viewportWidth": 1920
}
```

The property `ignoreTestFiles` in the example above it is used to ignore the default tests provided by Cypress.

It is also better to increase the default viewport which is to small.

| Name              | Value's description                   |
| ----------------- | ------------------------------------- |
| `baseURL`         | URL of the webpage to test            |
| `ignoreTestFiles` | Test spec files to be ignored         |

## Cypress project

The `cypress` folder has the following content:

```
cypress/
|--fixtures/
|--integration/
|--plugins/
|--support/
```

### Support

`index.js` is the very first file that cypress executes.

Add the commonly used functions in `command.js`.

### Plugins

`index.js` is the place to reference additional plugins.

### Integration

This diretory contains the all the test specifications. _Cypress runner_ looks into this folder and runs the test files.

### Fixture

Test data objects are stored here. Most of the time these are .json files with test data.

## Selection of elements

Elements are selected with the method `cy.get()`. Suppose you want to select the HTML element

```HTML
<input id="inputEmail1" placeholder="Email" class="input-full-width size-medium shape-rectangle" type="email" _ngcontent-bln-c22 data-cy="imputEmail1" />
```

Your Cypress test then writes:

```js
describe('test suite', () => {
    it('a test', () => {
        // Makes Cypress visit the website (which is located at the root of base URL)
        cy.visit('/');

        // select by tag
        cy.get('input')

        // select by ID
        cy.get('#inputEmail1')
        
        // select by class name
        cy.get('.input-full-width')
        
        // select by attribute name
        cy.get('[placeholder]')
        
        // select by attribute name and value
        cy.get('[placeholder="Email"]')
        
        // select by class value
        cy.get('[class="input-full-width size-medium shape-rectangle"]')

        // select by tag name and attribute with value
        cy.get('input[placeholder="Email"]')

        // select by several attributes
        cy.get('[placeholder="Email"][type="email][_ngcontent-nmg-c19]')

        // select by tag name, attribute with value, ID and class name
        cy.get('input[placeholder="Email"]#inputEmail1.input-full-width')

        /* THE MOST RECOMMANDED WAY BY CYPRESS 
        Assign specific cypress attribute to the HTML element
        */
       cy.get('[data-cy="imputEmail1"]')
        
    });
});
```

### get vs. find

The `get` method has a global context even if chained after another selection method. Method `find` must be chained after a valid selection method and looks for a child element of that selection.

## Assertions

### BDD assertions

The *Chai* BDD assertions are built with the structure:

```js
expect(<something>).to.<verb>(<value>)
```

### TDD assertions

They are value oriented and provide some interesting comparisons for numerical values.

### Chai-jQuery

These are special BDD assertions for HTML elements. These assertions are mostly built with the structure:

```js
expect(<$element>).to.<verb>.<property>(<value>)
```

### Sinon-Chai

Assertions for *stubs* and *spies*.

### Cypress <tt>should</tt> assertions

**Syntax**

```js
.should(chainers)
.should(chainers, value)
.should(chainers, method, value)
.should(callbackFn)
```

Here chainer is any valid chainer that comes from *Chai* or *Chai-jQuery* or *Sinon-Chai* without the leading `.to`.

When chained the `should` statements may be replaced by `and` (alias).

## Debugging

Besides the usual `console.log()`, Cypress provides its own console logging system with `cy.log()`.

It is possible to define breakpoints with the command `cy.pause()`.