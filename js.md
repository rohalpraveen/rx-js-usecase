## Call with this
- The call() allows for a function/method belonging to one object to be assigned and called for a different object.
- call() provides a new value of this to the function/method. With call(), you can write a method once and then inherit it in another object, without having to rewrite the method for the new object.

```ts
function AddNewPropstoThis(prop1, prop2) {
  this.smartProp = prop1 +'__'+ prop2;
}

function MyObj(prop1, prop2) {
  AddNewPropstoThis.call(this, prop1, prop2); // this code will add smartProp to MyObj #call(this)
  this.category = 'food';
}

console.log(new MyObj('ng', 'change').smartProp)

// expected output: "ng__changes"
```ts

## bind
- The bind() method creates a new function that, when called, has its this keyword set to the provided value, with a given sequence of arguments preceding any provided when the new function is called.

```ts
const module = {
  x: 42,
  getX: function(a) {
    console.log(a);
    return this.x;
  }
};

const unboundGetX = module.getX;
console.log(unboundGetX()); // The function gets invoked at the global scope
// expected output: undefined

const boundGetX = unboundGetX.bind(module, 'pro from arg');
console.log(boundGetX());
// expected output: 42

```ts

## Object vs Function call
```ts
'use strict'
function display() {
  this.prop1 = 'prohal'
  console.log('sData value is %s ', this.prop1);
}

new display(); //  sData value is prohal
display(); // cannot assign prop1 of undefined
display.call();
```ts
