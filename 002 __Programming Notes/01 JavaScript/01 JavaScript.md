# ~={red}1. Objects & Prototypes=~

1 key-Value pair

```js
let person = {
    firstName: 'John',
    lastName: 'Doe'
};

person.greet = function () {
    console.log('Hello!');
}

person.greet();

-------------------------------------------------------------------

let person = {
    firstName: 'John',
    lastName: 'Doe'
};

function greet() {
    console.log('Hello, World!');
}

person.greet = greet;

person.greet();


```

2. Object method shorthand

```js
let person = {
    firstName: 'John',
    lastName: 'Doe',
    greet: function () {
        console.log('Hello, World!');
    }
};

-----------------------------------------------------------------
let person = {
    firstName: 'John',
    lastName: 'Doe',
    greet() {
        console.log('Hello, World!');
    }
};

person.greet();
```

3. This 

```js
let person = {
    firstName: 'John',
    lastName: 'Doe',
    greet: function () {
        console.log('Hello, World!');
    },
    getFullName: function () {
        return this.firstName + ' ' + this.lastName;
    }
};


console.log(person.getFullName());
```

```
Output : "Josh Doe"
```

# 2. ~={red}Constructor Function=~

1. Constructor function

```js
function Person(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
}

let person = new Person('John','Doe');
```

2. Adding methods

```js
function Person(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;

    this.getFullName = function () {
        return this.firstName + " " + this.lastName;
    };
}

let person = new Person("John", "Doe");
console.log(person.getFullName());

// new keywork use for refrence to the Person function
```

---

# 3. Prototype
- Prototype is object from which another object can inherit properties and methods

```js
function User(name) {
    this.name = name;
}

User.prototype.greet = function () {
    console.log(`Hello, ${this.name}`);
};

const user1 = new User("Alex");
const user2 = new User("John");

user1.greet(); // Hello, Alex
user2.greet(); // Hello, John
```

1. Prototype Chaining 
	- Prototype chaining is the process of searching for a property or method in object's  prototype. Then continuing up the prototype chain until the property is found or the chain reaches null.

```js
const grandParent = {
    country: "India"
};

const parent = Object.create(grandParent);

parent.city = "Surat";

const child = Object.create(parent);

child.name = "Alex";

console.log(child.name);    // Alex
console.log(child.city);    // Surat
console.log(child.country); // India
```


2. `__proto__`
	- `__proto__` is property access that allow you to access or change object's internal prototype. 

```js
const parent = {
    name: "Parent",
    greet() {
        console.log("Hello!");
    }
};

const child = {
    age: 20
};

child.__proto__ = parent;

console.log(child.age);  // 20
console.log(child.name); // Parent

child.greet();           // Hello!
------------------------------------------------------------------------------------
child.__proto__ === parent; // true
```