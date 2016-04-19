# Javascript Design Patterns
Introduction to Javascript Design Patterns

In the early days of the web, Javascript acted mainly as an API to access the DOM. But as the web grew and the power of the Javascript langauage was recognized, common design patterns were applied to Javascript. 

Now, in order to be current with web best practices, it's important to know how to use Javascript in a way that is object oriented. 

The list of design patterns to use are:

- Constructor Pattern
- Module Pattern
- Revealing Module Pattern
- Singleton Pattern
- Observer Pattern
- Mediator Pattern
- Prototype Pattern
- Command Pattern
- Facade Pattern
- Factory Pattern
- Mixin Pattern
- Decorator Pattern
- Flyweight Pattern

Object Creation
---------------------

3 ways of creating objects in Javascript.

```javascript
// Each of the following options will create a new empty object:
 
var newObject = {};
 
// or
var newObject = Object.create( Object.prototype );
 
// or
var newObject = new Object();
```

And 4 ways to assign values to these objects.

```javascript
// 1. Dot syntax
 
// Set properties
newObject.someKey = "Hello World";
 
// Get properties
var value = newObject.someKey;
```
```javascript
// 2. Square bracket syntax
 
// Set properties
newObject["someKey"] = "Hello World";
 
// Get properties
var value = newObject["someKey"];
```
```javascript
// ECMAScript 5 only compatible approaches
// For more information see: http://kangax.github.com/es5-compat-table/
 
// 3. Object.defineProperty
 
// Set properties
Object.defineProperty( newObject, "someKey", {
    value: "for more control of the property's behavior",
    writable: true,
    enumerable: true,
    configurable: true
});
 
// If the above feels a little difficult to read, a short-hand could
// be written as follows:
 
var defineProp = function ( obj, key, value ){
  var config = {
    value: value,
    writable: true,
    enumerable: true,
    configurable: true
  };
  Object.defineProperty( obj, key, config );
};
 
// To use, we then create a new empty "person" object
var person = Object.create( Object.prototype );
 
// Populate the object with properties
defineProp( person, "car", "Delorean" );
defineProp( person, "dateOfBirth", "1981" );
defineProp( person, "hasBeard", false );
 
console.log(person);
// Outputs: Object {car: "Delorean", dateOfBirth: "1981", hasBeard: false}
```
```javascript
// 4. Object.defineProperties
 
// Set properties
Object.defineProperties( newObject, {
 
  "someKey": {
    value: "Hello World",
    writable: true
  },
 
  "anotherKey": {
    value: "Foo bar",
    writable: false
  }
 
});
```
Constructors
---------------------

Constructors are functions that act as classes. They are blueprints for objects.

```javascript
function Car( model, year, miles ) {
 
  this.model = model;
  this.year = year;
  this.miles = miles;
 
  this.toString = function () {
    return this.model + " has done " + this.miles + " miles";
  };
}
 
// Usage:
 
// We can create new instances of the car
var civic = new Car( "Honda Civic", 2009, 20000 );
var mondeo = new Car( "Ford Mondeo", 2010, 5000 );
 
// and then open our browser console to view the
// output of the toString() method being called on
// these objects
console.log( civic.toString() );
console.log( mondeo.toString() );
```

Constructors can be modified after their declaration by editing their prototype.

```javascript
function Car( model, year, miles ) {
 
  this.model = model;
  this.year = year;
  this.miles = miles;
 
}
 
 
// Note here that we are using Object.prototype.newMethod rather than
// Object.prototype so as to avoid redefining the prototype object
Car.prototype.toString = function () {
  return this.model + " has done " + this.miles + " miles";
};
 
// Usage:
 
var civic = new Car( "Honda Civic", 2009, 20000 );
var mondeo = new Car( "Ford Mondeo", 2010, 5000 );
 
console.log( civic.toString() );
console.log( mondeo.toString() );
```
Object Literals
---------------

Back to the first example of how to create objects. 

```javascript
var obj = {
    "example": "foo",
    "example2": "foo2",
    "example3": "foo3",
    editExample: function(str){
      this.example = str;
    }
};
obj.editExample("moo") // logs "moo"
```
Module Pattern
--------------

Modules are self invoking functions, but that return an object

A self invoking function

```javascript
(function(){
 console.log("I called myself");
})();
```

A self invoking function that returns an object, also known as a module

```javascript
var module = (function(){
  var privateVar = "foo";
  return {
    var publicVar : "moo"
  }
})();
console.log(module.privateVar) // undefined
console.log(module.publicVar) // moo
```

When a module includes internal functions for editing its private variables, it's using the revealing module pattern.

Importing variables into the self invoking function are called mixins

```javascript
var module = (function($){
  $("body").html(); // We can use jQuery now!
  return {}
})(jQuery);
```

Singletons
----------

Singletons are modules that point all instances to the same internal object

```javascript
var singleton = (function () {
   var instance;
  function init() {
    var privateRandomNumber = Math.random();
    return {
      getRandomNumber: function() {
        return privateRandomNumber;
      }
    };
  };
  return {
    createInstance: function () {
      if ( !instance ) {
        instance = init();
      }
      return instance;
    }
  };
})();

var obj1 = singleton.createInstance();
var obj2 = singleton.createInstance();

console.log(obj1.getRandomNumber()) // logs a number
console.log(obj2.getRandomNumber()) // logs the same number!
```
Observer
--------

The observer pattern refers to a series of objects called observers that update when data changes in another object called the subject.

In javascript a few classes are created that handle the observer subject relationships. 

Here are the Observer classes:
```javascript
function ObserverList(){
  this.observerList = [];
}
 
ObserverList.prototype.add = function( obj ){
  return this.observerList.push( obj );
};
 
ObserverList.prototype.count = function(){
  return this.observerList.length;
};
 
ObserverList.prototype.get = function( index ){
  if( index > -1 && index < this.observerList.length ){
    return this.observerList[ index ];
  }
};
 
ObserverList.prototype.indexOf = function( obj, startIndex ){
  var i = startIndex;
 
  while( i < this.observerList.length ){
    if( this.observerList[i] === obj ){
      return i;
    }
    i++;
  }
 
  return -1;
};
 
ObserverList.prototype.removeAt = function( index ){
  this.observerList.splice( index, 1 );
};
```
Here are the subject classes:
```javascript
function Subject(){
  this.observers = new ObserverList();
}
 
Subject.prototype.addObserver = function( observer ){
  this.observers.add( observer );
};
 
Subject.prototype.removeObserver = function( observer ){
  this.observers.removeAt( this.observers.indexOf( observer, 0 ) );
};
 
Subject.prototype.notify = function( context ){
  var observerCount = this.observers.count();
  for(var i=0; i < observerCount; i++){
    this.observers.get(i).update( context );
  }
};
```

Publish Subscribe | Event Aggregation
-----------------

The publish/subscribe pattern builds on the observer pattern by using event handlers between the observers and subjects, thus avoiding dependencies between the observers and subject.

Here several popular javascript libraries have their implementation of the publish subscribe pattern.

```javascript
// Publish
 
// jQuery: $(obj).trigger("channel", [arg1, arg2, arg3]);
$( el ).trigger( "/login", [{username:"test", userData:"test"}] );
 
// Dojo: dojo.publish("channel", [arg1, arg2, arg3] );
dojo.publish( "/login", [{username:"test", userData:"test"}] );
 
// YUI: el.publish("channel", [arg1, arg2, arg3]);
el.publish( "/login", {username:"test", userData:"test"} );
 
 
// Subscribe
 
// jQuery: $(obj).on( "channel", [data], fn );
$( el ).on( "/login", function( event ){...} );
 
// Dojo: dojo.subscribe( "channel", fn);
var handle = dojo.subscribe( "/login", function(data){..} );
 
// YUI: el.on("channel", handler);
el.on( "/login", function( data ){...} );
 
 
// Unsubscribe
 
// jQuery: $(obj).off( "channel" );
$( el ).off( "/login" );
 
// Dojo: dojo.unsubscribe( handle );
dojo.unsubscribe( handle );
 
// YUI: el.detach("channel");
el.detach( "/login" );
```
Create your own pub/sub classes:
```javascript
var pubsub = {};
 
(function(myObject) {
 
    // Storage for topics that can be broadcast
    // or listened to
    var topics = {};
 
    // An topic identifier
    var subUid = -1;
 
    // Publish or broadcast events of interest
    // with a specific topic name and arguments
    // such as the data to pass along
    myObject.publish = function( topic, args ) {
 
        if ( !topics[topic] ) {
            return false;
        }
 
        var subscribers = topics[topic],
            len = subscribers ? subscribers.length : 0;
 
        while (len--) {
            subscribers[len].func( topic, args );
        }
 
        return this;
    };
 
    // Subscribe to events of interest
    // with a specific topic name and a
    // callback function, to be executed
    // when the topic/event is observed
    myObject.subscribe = function( topic, func ) {
 
        if (!topics[topic]) {
            topics[topic] = [];
        }
 
        var token = ( ++subUid ).toString();
        topics[topic].push({
            token: token,
            func: func
        });
        return token;
    };
 
    // Unsubscribe from a specific
    // topic, based on a tokenized reference
    // to the subscription
    myObject.unsubscribe = function( token ) {
        for ( var m in topics ) {
            if ( topics[m] ) {
                for ( var i = 0, j = topics[m].length; i < j; i++ ) {
                    if ( topics[m][i].token === token ) {
                        topics[m].splice( i, 1 );
                        return token;
                    }
                }
            }
        }
        return this;
    };
}( pubsub ));
```

Mediator
--------

When components refer to each other directly they are considered to be coupled, which is bad for scaling. Decoupling components allows for code reusability. This is often where a mediator comes in.
