## 1. Factory
С помощию паттерт Factory, мы можем создать очень просто нового участника. Используя в этом случае только класс, MemberFactory

```
class SimpleMembership {
  name: string;
  cost: number;
  constructor(name: string) {
    this.name = name;
    this.cost = 50;
  }
}

class StandardMembership {
  name: string;
  cost: number;
  constructor(name: string) {
    this.name = name;
    this.cost = 150;
  }
}

class PremiumMembership {
  name: string;
  cost: number;
  constructor(name: string) {
    this.name = name;
    this.cost = 150;
  }
}

type TypeMembership = 'simple' | 'standard' | 'premium';

class MemberFactory {
  private membership = {
      simple: SimpleMembership,
      standard: StandardMembership,
      premium: PremiumMembership,
  };
  create(name: string, type: TypeMembership) {
      const Membership = this.membership[type];
      return new Membership(name);

  }
}

const factory = new MemberFactory();

const John = factory.create('John', 'simple');
const Sally = factory.create('Sally', 'premium');
```

## 2. Singleton
Очень полезный паттерн, если нам надо создать одно соединение в базу данных. Мы просто будем проверять instance класса, если существует, соедение созданно.
```
class Database {
  static instance: any;
  private data!: string;
  constructor(data: string) {
    if (!Database.instance) {
      Database.instance = this;
      this.data = data;
      return;
    }
    return Database.instance;
  }

  getData() {
    return this.data;
  }
}

const prisma = new Database('PrismaORM');
const typeorm = new Database('TypeORM');

console.log(prisma.getData()); //PrismaORM
console.log(typeorm.getData()); //PrismaORM

```
## 3. Decorator
C помощью этого паттерна. Мы можем изменить класс (нам мне придется писать EspressoWithSugar, Espresso)
И динамически изменить, от наших предпочтений.

```
class Coffee {
  withSugar: boolean;
  cost: number;
  name: string;
  constructor(withSugar: boolean, cost: number, name: string) {
    this.withSugar = withSugar;
    this.name = name;
    this.cost = cost;
  }

  getAllData() {
    return this;
  }
}

function withSugar(coffee: Coffee, price: number) {
  coffee.withSugar = true;
  coffee.cost += price;
  return coffee;
}

const coffee = withSugar(new Coffee(false, 5, 'americano'), 4);
console.log(coffee);
```
## 4. Facade 
Можем использовать только класс ComplaintRegistr и создавать жалобы, не заботясь как это уже устоеено внутри.

```
class Complaints {
  constructor() {
    this.complaints = []
  }

  reply(complaint) {}

  add(complaint) {
    this.complaints.push(complaint)
    return this.reply(complaint)
  }
}

class ProductComplaints extends Complaints {
  reply({id, customer, details}) {
    return `Product: ${id}: ${customer} (${details})`
  }
}

class ServiceComplaints extends Complaints {
  reply({id, customer, details}) {
    return `Service: ${id}: ${customer} (${details})`
  }
}

class ComplaintRegistry {
  register(customer, type, details) {
    const id = Date.now()
    let complaint

    if (type === 'service') {
      complaint = new ServiceComplaints()
    } else {
      complaint = new ProductComplaints()
    }

    return complaint.add({id, customer, details})
  }
}

const registry = new ComplaintRegistry()

console.log(registry.register('Alex', 'service', 'недоступен'))
console.log(registry.register('Elena', 'product', 'вылазит ошибка'))
```
## 5. Strategy

В зависимости он скидки, мы можем седелать расчет суммы в зависимости от этой кидки без всяких if-ов.

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

Наш UserService может получить инстанс UserRepository и рабоать с ним, это помогает там с контистентности данных(данные будут изменяться предсказуемым образом)

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
Мы с помощью Observer подписываемся с разных устройств (Mobile, Web) и при получение уведомление. Оно рассылается по все устройствам.

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
