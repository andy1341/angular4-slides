# Type script


## What is TypeScript?
TypeScript is a typed superset of JavaScript compiled to JavaScript. In other words, TypeScript is JavaScript plus some additional features. <!-- .element: class="fragment" -->
![asd](img/type_script/es-ts.png)<!-- .element: class="fragment" -->


## Why Use TypeScript?
- Compilation
- Strong Static Typing
- Type definitions
- Decorators
- Object Oriented Programming


## Types
The major improvement of TypeScript over ES6, that gives the language its name, is the typing system.
You might be a little skeptical of type checking but I’d encourage you to give it a chance.
One of the great things about type checking is that
- It helps when writing code because it can prevent bugs at compile time
- It helps when reading code because it clarifies your intentions


## Declaring types
The new TypeScript syntax is a natural evolution from ES5, we still use var but now we can optionally provide the variable type along with its name:
```ts
var fullName: string;
```
When declaring functions we can use types for arguments and return values:
```ts
function greetText(name: string): string {
  return "Hello " + name;
}
```


## Built-in types
- String
```ts
var fullName: string = 'Nate Murray';
```
- Number
```ts
var age: string = 3;
```
- Boolean
```ts
var married: boolean = false;
```
- Array
```ts
var jobs: Array<string> = ['IBM', 'HP'];
var jobs: string[] = ['Dell', 'HP'];
```


## Built-in types
- Enums
```ts
enum Role {Employee, Manager, Admin};
var role: Role = Role.Employee;
```
- Any
```ts
var something: any = 'as string';
something = 1;
something = [1, 2, 3];
```
- Void
```ts
function setName(name: string): void {
  this.fullName = name;
}
```


## Interfaces
One of TypeScript’s core principles is that type-checking focuses on the shape that values have.
In TypeScript, interfaces fill the role of naming these types, and are a powerful
way of defining contracts within your code as well as contracts with code outside of your project.


## Interfaces
```ts
interface PersonInterface {
  first_name: string;
  last_name: string;
}

var person: PersonInterface = {
  first_name: 'Elizabet'
} // wrong, person should have last_name

var person: PersonInterface = {
  first_name: 'Elizabet',
  last_name: 'Second'
} // right
```


## Classes
To define a class we use the new class keyword and give our class a name and a body:
```ts
class Person {
}
```
Classes may have properties, methods, and constructors.


## Class properties
Properties define data attached to an instance of a class. For example, a class named `Person` might have properties like `first_name` and `last_name`.

```ts
class Person {
  first_name: string;
  last_name: string;
}
```


## Methods
Methods are functions that run in context of an object. If we wanted to add a way
to greet a Person using the class above, we would write something like:
```ts
class Person {
     first_name: string;
     last_name: string;

     greet(): void {
       return "Hello, " + this.first_name;
     }
   }
```


## Constructors
A constructor is a special method that is executed when a new instance of the class is being created.
Usually, the constructor is where you perform any initial setup for new objects.
```ts
class Person {
  first_name: string;
  last_name: string;

  constructor(first_name: string, last_name: string) {
    this.first_name = first_name;
    this.last_name = last_name;
  }

  greet(): void {
    return "Hello, " + this.first_name;
  }
}
```


## Inheritance
Another important aspect of object oriented programming is inheritance. Inheritance is a way to
indicate that a class receives behavior from a parent class. Then we can override, modify or augment
those behaviors on the new class.

```ts
class Worker extends Person {
  position: string;

  constructor(first_name: string, last_name: string, position: string) {
    super(first_name, last_name)
    this.position = position
  }

  whoAreYou(): string {
    return `I am ${this.position}`;
  }
}
```


## Usage
```ts
  var person = new Person('Marcus', 'Cruz')
  var worker = new Worker('Angela', 'Walker', 'Sales Manager')

  person.greet(); // Hello, Marcus
  worker.greet(); // Hello, Angela
  peson.whoAreYou(); // undefined method
  worker.whoAreYou(); // I am Sales Manageer
```


## Decorators
A Decorator is a special kind of declaration that can be attached to a class declaration,
method, accessor, property, or parameter. Decorators use the form @expression,
where expression must evaluate to a function that will be called at runtime
with information about the decorated declaration.
```ts
@Fullname()
class Person
{
  ...
}
```


## Class decorators
```ts
@Fullname({})
class Person {
  ...
}

function Fullname(options) {
    return (target) => {
        target.fullname = () => {
            return `${this.first_name} ${this.last_name}`
        }
    }
}
```
