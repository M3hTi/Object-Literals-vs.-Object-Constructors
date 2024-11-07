# Object Literals vs. Object Constructors

## Object Literals
- Represent a single, unique object
- Created directly using `{}` syntax
- Properties and methods defined directly within the object
- Good for one-time, specific objects

Example:
```javascript
const person1 = {
    name: "John",
    age: 30,
    greet() {
        return `Hello, I'm ${this.name}`;
    }
};
```

## Object Constructors
- Serve as a reusable template for creating multiple similar objects
- Defined using a function with the `new` keyword to create instances
- Properties and methods can be defined within the constructor function or on the prototype
- Better for creating multiple, related objects

Example:
```javascript
function Person(name, age) {
    this.name = name;
    this.age = age;
}

Person.prototype.greet = function() {
    return `Hello, I'm ${this.name}`;
};

const person2 = new Person("Alice", 25);
const person3 = new Person("Bob", 35);
```

# Object Composition

In JavaScript, object composition is a technique where an object can include objects defined in other classes. This allows for the creation of complex objects by combining simpler, more modular components.

## Example: Composing a `Car` Object

```javascript
class Wheel {
    constructor(size) {
        this.size = size;
    }
}

class Engine {
    constructor(type) {
        this.type = type;
    }

    start() {
        return `${this.type} engine started`;
    }
}

class Radio {
    constructor(brand) {
        this.brand = brand;
    }

    playMusic() {
        return "Playing music";
    }
}

class Car {
    constructor(model, type, brand, size) {
        this.model = model;
        this.engine = new Engine(type);
        this.radio = new Radio(brand);
        this.wheels = [
            new Wheel(size),
            new Wheel(size),
            new Wheel(size),
            new Wheel(size)
        ];
    }
}

const myCar = new Car("BMW", "V8", "Panasonic", 20);
console.log(myCar.engine.start());     // "V8 engine started"
console.log(myCar.radio.playMusic());  // "Playing music"
console.log(myCar.wheels.length);      // 4
```

In this example, the `Car` class is composed of several other classes (`Wheel`, `Engine`, and `Radio`). The `Car` constructor creates instances of these other classes and stores them as properties of the `Car` object, allowing the `Car` to access the functionality of its composited components.

# Prototype Inheritance

JavaScript uses prototype-based inheritance, where objects inherit properties and methods from a prototype object. This allows for more efficient memory usage and code reuse.

## Example: Implementing Inheritance with Prototypes

```javascript
function Animal(name) {
    this.name = name;
}

Animal.prototype.makeSound = function() {
    console.log("The animal makes a sound");
};

function Dog(name, breed) {
    Animal.call(this, name); // Call the parent constructor
    this.breed = breed;
}

Dog.prototype = Object.create(Animal.prototype); // Inherit from the parent
Dog.prototype.constructor = Dog; // Fix the constructor reference

const myDog = new Dog("Buddy", "Labrador");
myDog.makeSound(); // "The animal makes a sound"
```

In this example, the `Dog` constructor inherits from the `Animal` constructor by setting the `Dog.prototype` to an instance of `Animal.prototype`. This allows instances of `Dog` to use the `makeSound()` method defined on the `Animal` prototype.

# Public, Private, and Privileged Methods

JavaScript allows you to create different levels of access control for your object methods:

1. **Public Methods**: Accessible both inside and outside the object instance.
2. **Private Methods**: Accessible only within the constructor function, not from outside the object instance.
3. **Privileged Methods**: Accessible both inside and outside the object instance, but can also access private variables and methods.

## Example: Implementing Encapsulation

```javascript
function Person(fName, lName, age) {
    this.firstName = fName;
    this.lastName = lName;
    this.age = age;

    const info = function() {
        return `My name is ${this.firstName} ${this.lastName} and I'm ${this.age} years old`;
    };

    this.getInfo = function() {
        return info.call(this);
    };
}

const person1 = new Person("Ali", "mohammadi", 30);
console.log(person1.getInfo()); // Output: "My name is Ali mohammadi and I'm 30 years old"
```

In this example:

- `info()` is a private method, accessible only within the `Person` constructor function.
- `getInfo()` is a privileged method, as it can access the private `info()` method and is also accessible from outside the `Person` object.

By using this pattern, you can effectively encapsulate the internal implementation details of your objects while still providing a controlled public interface for interacting with them.