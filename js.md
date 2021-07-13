## Call with this

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
