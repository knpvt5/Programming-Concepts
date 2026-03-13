# **The `this` Keyword**

- The `this` keyword in JavaScript refers to the context in which a function is executed. **Its value can change based on how and where the function is called.**

## **Global Context**

- In the global context (outside of any function), `this` refers to the global object.
- In browsers, the global object is `window`.
- Example:

```js
console.log(this); // In a browser, logs the Window object

this = window //true
```

## **Function Context**

- In a function context, `this` refers to the object that is calling the function.
- If a function is called as a method of an object, `this` refers to that object.
- Example:

```js
const obj = {
  name: "karan",
  greet: function () {
    console.log("Hello, " + this.name);
  },
};

obj.greet(); // Logs: "Hello, karan"
```

```js
const user = {
  firstName: "karan",
  lastName: "don",
  fullName() {
    function getAge() {
      console.log("Getting Age");
      console.log(this);
    }
    getAge();
  },
};

user.fullName();

//here the this in getAge, again point to window, becuase it is called inside funciton and not in object directly
```

- If a function is called without an object context, `this` refers to the global object (or `undefined` in strict mode).

```js
const user = {
  name: "karan",
  nums: [1, 2, 3],
  print() {
    this.nums.forEach(function (n) {
      console.log(this.name, n);
    }, this); // thisArg
  },
};

user.print();

// In array method like foreach, we have to explicitly make the 'this' to point to the object where this array is been used. bydefualt is points to window object
```

```js
function show() {
  console.log(this);
}
show(); // In non-strict mode, logs the global/window object; in strict mode, logs undefined

new show(); //    in constructor function , when we call function with new keyword, it alway creates new object and then retruns the new object, now this keyword in constructor function refer this return object
```

## **Arrow Functions**

- Arrow functions do not have their own `this` context. Instead, they inherit `this` from the parent scope at the time they are defined.
- Example:

```js
const obj = {
  name: "karan",
  greet: function () {
    setTimeout(() => {
      console.log("Hello, " + this.name);
    }, 1000);
  },
};

obj.greet(); // Logs: "Hello, karan" after 1 second
```

JavaScript’s this is contextual and sometimes confusing - that's why arrow functions were introduced (they bind this lexically)

## **Explicit Binding**

```js
const obj = {
  name: "karan",
};

function greet(msg) {
  console.log("Hello, " + this.name + ". " + msg);
}

greet.call(obj, "Welcome!"); // Logs: "Hello, karan. Welcome!"
greet.apply(obj, ["Welcome!"]); // Logs: "Hello, karan. Welcome!"
const boundGreet = greet.bind(obj);
boundGreet(); // Logs: "Hello, karan"
```

- `call` and `apply` invoke the function immediately with a specified `this` context.
  - call() takes arguments individually
  - apply() takes arguments as an array
- `bind` returns a new function that has `this` permanently set to the given value (obj).
  - .bind() does not call the function immediately instead The bound function can be called later.

## **Class Context**

- In a class context, `this` refers to the instance of the class.
- Example:

```js
class Person {
  constructor(name) {
    this.name = name;
  }

  greet() {
    console.log("Hello, " + this.name);
  }
}

const karan = new Person("karan");
karan.greet(); // Logs: "Hello, karan"

// When we pass a method as a callback/reference, this can be lost⬇️:
const greetFn = karan.greet;
greetFn(); // Logs: "Hello, undefined"
setTimeout(karan.greet, 1000); // Logs: "Hello, undefined" after 1 second
// ℹ️To fix this, we use Explicit Binding like call, apply or bind
```

- When using `this` in classes, it refers to the instance created by the class constructor.
