# sinon-cheatsheet
Most used sinon operations
* [Setup](#setup)
* [Normal functions](#normal-functions)
  * [Expect function to throw error (function not using this)](#expect-function-to-throw-error-function-not-using-this)
  * [Expect function to throw error (function using this)](#expect-function-to-throw-error-function-using-this)
* [Promises](#promises)
  * [Expect function(promise) to throw (reject) with error](#expect-function-to-throw-reject-with-error)

## Setup
```javascript
const chai = require('chai');
var chaiAsPromised = require("chai-as-promised");
chai.should();
chai.use(chaiAsPromised);

const assert = chai.assert;
const expect = chai.expect;

// Sinon is a library used for mocking or verifying function calls in JavaScript.
const sinon = require('sinon');
```

## NORMAL FUNCTIONS
### Expect function to throw error (function not using this)
```javascript

var objectUnderTest = {
  testFunction: function(){
    throw new Error('Something went wrong');
  }
}

// now going to test, important notice - this is not set when invoked as below

it('My Test', () => {     
  return expect(objectUnderTest.testFunction).to.throw('Something went wrong');                      
})

```

### Expect function to throw error (function using this)
```javascript

var objectUnderTest = {
  code: 100;
  testFunction: function(){
    throw new Error('Something went wrong for code: '+this.code);
  }
}

// now going to test, notice using bind() - as a result *this* inside function will be set accordingly so the code will behave as expected

it('My Test', () => {     
  return expect(objectUnderTest.testFunction.bind(objectUnderTest)).to.throw('Something went wrong for code: '+objectUnderTest.code);                      
})

```

## PROMISES
### Expect function(promise) to throw (reject) with error
```javascript

var objectUnderTest = {
  code: 100;
  testFunction: function(){
  var self = this;
    return new Promise(function(resolve, reject){
      throw new Error('Something went wrong for code: '+self.code);
    })
  }
}

// now going to test, now this will be set accordingly so the code will behave as expected

it('My Test', () => {     
  return objectUnderTest.testFunction().should.be.rejectedWith('Something went wrong for code: '+objectUnderTest.code);                      
})

```

