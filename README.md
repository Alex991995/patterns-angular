# Types of patterns:

## 1. Creational
Provides object creation mechanisms that increase flexibility and reuse of existing code.

## 2. Structural
Explains how to assemble objects and classes into larger structures, while keeping these structures flexible and efficient.

## 3. Structural
Takes care of effective communication and the assignment of responsibilities between objects.



# Useful design patterns

## 1. Factory
Provides an interface for creating objects without explicitly specifying their concrete classes

```
class IOSButton {}
class AndroidButton {}

class ButtonFactory {
  createButton(os: string): IOSButton | AndroidButton {
    if (os === 'ios') {
      return new IOSButton();
    }
    return new AndroidButton();
  }
}

const factory = new ButtonFactory()
const btnIOS = factory.createButton('ios')
const btnAndroid = factory.createButton('android')
```

## 2. Singleton
Ensures a class has only one instance and provides a global point of access to it.
```
class Singleton {
  constructor() {
    if (!Singleton.instance) {
      Singleton.instance = this;
    }
    return Singleton.instance;
  }
}
const instance1 = new Singleton();
const instance2 = new Singleton();
console.log(instance1 === instance2); // true
```
## 3. Decorator
Decorator pattern is about adding responsibilities to objects dynamically

```
function greet(name) {
  return `Hello, ${name}!`;
}

function withLogging(fn) {

  return function(...args) {
    const result = fn(...args);
    return result;
  };
}

const greetWithLogging = withLogging(greet);
greetWithLogging('John'); 

// Returns: "Hello, John!"
```
## 4. Facade 
Provides a simplified interface to a library, a framework, or any other complex set of classes.

```
class SubsystemA {
  method() {
    console.log('This is a method of Subsystem-A');
  }
}

class SubsystemB {
  method() {
    console.log('This is a method of Subsystem-B');
  }
}

class SubsystemC {
  method() {
    console.log('This is a method of Subsystem-C');
  }
}

class Facade {
  constructor() {
    this.subsystemA = new SubsystemA();
    this.subsystemB = new SubsystemB();
    this.subsystemC = new SubsystemC();
  }

  commonInterface() {
    this.subsystemA.method();
    this.subsystemB.method();
    this.subsystemC.method();
  }
}
```
## 5. Strategy

Behavioral design pattern that enables the dynamic selection of algorithms at runtime.

```
interface DiscountStrategy {
  calculate(price: number): number;
}

class SeasonalDiscountStrategy implements DiscountStrategy {
  calculate(price: number): number {
    return price - price * 0.1;
  }
}

class PersonalDiscountStrategy implements DiscountStrategy {
  calculate(price: number): number {
    return price - price * 0.2;
  }
}

class Product {
  name: string;
  price: number;
  discount: DiscountStrategy | null = null;
  constructor(name: string, price: number, discount: DiscountStrategy | null) {
    this.name = name, 
    this.price = price,
    his.discount = discount,
  }
  getDiscountedPrice() {
    if (!this.discount) {
      return this.price;
    }
    return this.discount.calculate(this.price);
  }
}

const product = new Product('MacBook', 2000, new PersonalDiscountStrategy());
product.getDiscountedPrice(); //1600
```
## 6. Dependency Injection

Programming pattern in which a dependency is passed using the parameters instead of instantiating it within the function or class.

```
 class UserService {
  constructor(userRepository) {
    this.userRepository = userRepository;
  }

  getUser(id) {
    return this.userRepository.findById(id);
  }
}


const myUserRepository = new UserRepository();
const userService = new UserService(myUserRepository);
```

## 7. Observer
Behavioral design pattern that establishes a one-to-many dependency between objects. 

```
class Subject {
  constructor() {
    this.observers = [];
  }

  subscribe(observer) {
    this.observers.push(observer);
  }

  unsubscribe(observer) {
    this.observers = this.observers.filter(obs => obs !== observer);
  }

  notify(data) {
    this.observers.forEach(observer => observer.update(data));
  }
}

class Observer {
  constructor(name) {
    this.name = name;
  }

  update(data) {
    console.log(`${this.name} received update: ${data}`);
  }
}

const newsSubject = new Subject();

const mobileApp = new Observer('Mobile App');
const webDashboard = new Observer('Web Dashboard');

newsSubject.subscribe(mobileApp);
newsSubject.subscribe(webDashboard);
newsSubject.notify('Breaking News: Stock Market Soars!');
newsSubject.unsubscribe(mobileApp); 
newsSubject.notify('Weather Update: Sunny skies expected.');
```
## Additional

*  SOLID – A set of 5 principles for writing maintainable and scalable object-oriented code:

    * S: Single Responsibility Principle – A class should have only one reason to change.

    *  O: Open/Closed Principle – Software entities should be open for extension but closed for modification.

    *  L: Liskov Substitution Principle – Subtypes must be substitutable for their base types.

    *  I: Interface Segregation Principle – Prefer many small, specific interfaces over one large general-purpose interface.

    *  D: Dependency Inversion Principle – Depend on abstractions, not on concrete implementations.

* KISS (Keep It Simple, Stupid) – Design should be as simple as possible; avoid unnecessary complexity.

* DRY (Don’t Repeat Yourself) – Avoid code duplication; abstract and reuse logic instead.

* YAGNI (You Aren’t Gonna Need It) – Don’t implement something until it is actually needed.
