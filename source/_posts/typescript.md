---
title: typescript记录之Handbook(Basic Types、Interfaces、Classes)
---

本篇文章主要用于记录之用，从头到尾读一下 ts 文档，并记录下其中需要学习的东西，
毕竟里面有许多是不需要看的，像一些基本类型定义、或是 es6 已经很常用的一些语法，
记录这些，大概也能对进一步学习 ts 有个较好的作用，而不是每次翻开文档，
从头看个几页，下次又从头开始，大致从 handbook 和 what new 两部分开始，
因考虑到内容较长，分多篇

## Handbook

### Basic Types

#### never 类型

never 类型代表永不会发生的类型，大多用于表示函数返回值，如

```javascript
// Function returning never must have unreachable end point
function error(message: string): never {
  throw new Error(message);
}

// Inferred return type is never
function fail() {
  return error("Something failed");
}

// Function returning never must have unreachable end point
function infiniteLoop(): never {
  while (true) {}
}
```

#### 类型断言

某些时候，你可能比 ts 更了解某个类型的值，这个时候可以使用类型断言，类型断言有两种方式

```javascript
let someValue: any = "this is a string";
let strLength: number = (<string>someValue).length;

let someValue: any = "this is a string";
let strLength: number = (someValue as string).length;
```

因为 jsx 的普遍使用，第二种方式更为推荐

### Interfaces

#### Indexable Types

索引类型有一个索引签名用来描述，我们用什么类型对这个对象索引，以及使用索引时返回的内容类型，例如：

```javascript
interface StringArray {
  [index: number]: string;
}
```

上述是用来表示一个用数字为索引，返回值为 string 的对象，即 string[],
索引签名只有两种类型，string/number，但是这两种索引类型是不能共用的，因为 number 类型和 string 共用时，只能使用对象来表示，number 会被转为 string

#### 类对接口的实现

使用 implements 关键字可以表示类对接口的实现

```javascript
interface ClockInterface {
  currentTime: Date;
  setTime(d: Date): void;
}

class Clock implements ClockInterface {
  currentTime: Date = new Date();
  setTime(d: Date) {
    this.currentTime = d;
  }
  constructor(h: number, m: number) {}
}
```

一个 class 有两种类型，一种是静态端，一种是实例端，construct 函数式属于静态端的，所以下面内容会报错

```javascript
interface ClockConstructor {
    new (hour: number, minute: number);
}

class Clock implements ClockConstructor {
    currentTime: Date;
    constructor(h: number, m: number) { }
}
```

一个简单的办法是

```javascript
interface ClockConstructor {
  new (hour: number, minute: number);
}

interface ClockInterface {
  tick();
}

const Clock: ClockConstructor = class Clock implements ClockInterface {
  constructor(h: number, m: number) {}
  tick() {
      console.log("beep beep");
  }
}
```

接口可以继承多个接口

```javascript
interface Shape {
    color: string;
}

interface PenStroke {
    penWidth: number;
}

interface Square extends Shape, PenStroke {
    sideLength: number;
}

let square = <Square>{};
square.color = "blue";
square.sideLength = 10;
square.penWidth = 5.0;

```

#### Hybrid Types

可以描述一个既是函数又是对象的对象

```javascript
interface Counter {
    (start: number): string;
    interval: number;
    reset(): void;
}

function getCounter(): Counter {
    let counter = <Counter>function (start: number) { };
    counter.interval = 123;
    counter.reset = function () { };
    return counter;
}

let c = getCounter();
c(10);
c.reset();
c.interval = 5.0;
```

#### Interfaces Extending Classes

接口继承一个 class 的时候，会继承它的所有属性，包含 private 和 protected 的属性。这意味着如果你去实现一个继承了含 private 和 protected 属性的 class 的接口，只能通过这个 class 或者它的子类。这在你要使用一些有固定属性的子类时比较有用，这些子类除了继承自同一个父类外，不用有其他的联系。

```javascript
class Control {
    private state: any;
}

interface SelectableControl extends Control {
    select(): void;
}

class Button extends Control implements SelectableControl {
    select() { }
}

// Error: Property 'state' is missing in type 'Image'.
class Image implements SelectableControl {
    private state: any;
    select() { }
}
```

上例中，Button 的校验是通过的，因为它的 state 属性继承自 Control，而 Image 是不通过的，因为它的 state 属性与 SelectableControl 要求的不是同一来源，在属性为 private 和 protected 时，来源的校验是必须的。

### Classes

类的定义大多基于 es 标准，在这里不过多赘述，下面仅记录一些不常见或是 ts 特有的语法

#### Parameter properties

参数属性是一种并不那么直观的语法糖，适用于 public, protected,private,readonly
如下两种写法是等同的

```javascript
class Octopus {
    readonly name: string;
    readonly numberOfLegs: number = 8;
    constructor (theName: string) {
        this.name = theName;
    }
}
class Octopus {
    readonly numberOfLegs: number = 8;
    constructor(readonly name: string) {
    }
}
```

#### Abstract Classes

抽象类是可以从中派生其他类的基类。它们可能无法直接实例化。与接口不同，抽象类可以包含其部分成员的实现。abstract 关键字用于定义抽象类以及抽象类中的抽象方法。
抽象类中标记为抽象的方法不包含实现，必须在派生类中实现。抽象方法与接口方法语法类似。
两者都定义方法的签名而不包括方法体。但是，抽象方法必须包含 abstract 关键字，并且可以选择包含访问修饰符。

```javascript
abstract class Department {

    constructor(public name: string) {
    }

    printName(): void {
        console.log("Department name: " + this.name);
    }

    abstract printMeeting(): void; // must be implemented in derived classes
}

class AccountingDepartment extends Department {

    constructor() {
        super("Accounting and Auditing"); // constructors in derived classes must call super()
    }

    printMeeting(): void {
        console.log("The Accounting Department meets each Monday at 10am.");
    }

    generateReports(): void {
        console.log("Generating accounting reports...");
    }
}

let department: Department; // ok to create a reference to an abstract type,其实Department在此相当于一个interface，需要赋值department能implements Department的类，故Department的子类也可以，甚至其他属性一样的类
department = new Department(); // error: cannot create an instance of an abstract class
department = new AccountingDepartment(); // ok to create and assign a non-abstract subclass
department.printName();
department.printMeeting();
department.generateReports(); // error: method doesn't exist on declared abstract type

```

#### Advanced Techniques

当我们在 ts 中声明 class 的时候，它做了两个声明，第一个是 class 的实例类型

```javascript
class Greeter {
  greeting: string;
  constructor(message: string) {
    this.greeting = message;
  }
  greet() {
    return "Hello, " + this.greeting;
  }
}

let greeter: Greeter;
greeter = new Greeter("world");
console.log(greeter.greet());
```

像上述代码一样，**let greeter: Greeter;**，这里面的**Greeter**代表着类的实例类型。对于来自其他面向对象语言的程序员来说，这几乎是第二天性。第二个是创造了一个构造函数。即下面这段代码

```javascript
let Greeter = (function() {
  function Greeter(message) {
    this.greeting = message;
  }
  Greeter.prototype.greet = function() {
    return "Hello, " + this.greeting;
  };
  return Greeter;
})();

let greeter;
greeter = new Greeter("world");
console.log(greeter.greet());
```

**let Greeter**即是在声明构造函数，class 的另一种说法就是它包含静态端和实例端

```javascript
class Greeter {
  static standardGreeting = "Hello, there";
  greeting: string;
  greet() {
    if (this.greeting) {
      return "Hello, " + this.greeting;
    } else {
      return Greeter.standardGreeting;
    }
  }
}

let greeter1: Greeter;
greeter1 = new Greeter();
console.log(greeter1.greet()); // Hello, there

let greeterMaker: typeof Greeter = Greeter;
greeterMaker.standardGreeting = "Hey there!";

let greeter2: Greeter = new greeterMaker();
console.log(greeter2.greet()); // Hey there!
```

在上面的例子中，**greeterMaker**这个变量即是 class 自身，或者说构造函数。我们使用**typeof Greeter**来获取 class 自身而不是它的实例，**值得注意的是，greeterMaker 这个变量被赋值是按引用传递，而不是按值，所以它改变了整个 Greeter，接下来使用 Greeter 创建的实例都将 Hey there!**

#### Using a class as an interface

和上面说的一样，类的声明创造了两个东西，一是代表着类的实例的类型定义，一是 constructor function。因为类创造了类型定义，所以你可以把它当做接口来用

```javascript
class Point {
  x: number;
  y: number;
}

interface Point3d extends Point {
  z: number;
}

let point3d: Point3d = { x: 1, y: 2, z: 3 };
```
