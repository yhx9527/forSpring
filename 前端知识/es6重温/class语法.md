# class语法

- construction为构造方法，通过new生成实例即自动调用该方法

- 类中所有方法都定义在prototype上

- 类中定义的方法不可枚举

```
class Point {
  constructor(x, y) {
    // ...
  }

  toString() {
    // ...
  }
}

Object.keys(Point.prototype)
// []
Object.getOwnPropertyNames(Point.prototype)
// ["constructor","toString"]
```



- 类必须使用new创建
- 实例属性得定义在this上，否则就是在prototype上的
- get取值函数和set存值函数设置在了属性的descriptor对象上

```
class Point {
  constructor(x, y) { 
    this.x = x;
    this.y = y;
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }
  get prop() {
    return 'getter';
  }
  set prop(value) {
    console.log('setter: '+value);
  }
  //可用表达式作为属性名
  [methodName](){
  
  }
  //generate方法
  * [Symbol.iterator]() {
    for (let arg of this.args) {
      yield arg;
    }
  }
}
let p=new Point(1,2);
p.prop = 123;
// setter: 123

p.prop
// 'getter'
```

**立即执行的类名**

```
let person = new class {
  constructor(name) {
    this.name = name;
  }

  sayName() {
    console.log(this.name);
  }
}('张三');

person.sayName(); // "张三"
```

- 类的声明不存在变量提升
- 实例属性除了写在constructor中，也可以

```
class foo {
  bar = 'hello';
  baz = 'world';

  constructor() {
    // ...
  }
}
```



#### 静态方法

- 通过类来调用的方法，不会被继承

- 静态方法中的this指向类
- 父类静态方法可被子类继承，子类通过super表示父类

```
class Foo {
  static classMethod() {
    return 'hello';
  }
}

Foo.classMethod() // 'hello'

var foo = new Foo();
foo.classMethod()
// TypeError: foo.classMethod is not a function
```

#### 静态属性(提案中)

```
class Foo {
  static prop = 1;
}
```

#### 私有属性，私有方法(提案中)

用#标识

```
class Foo {
  #a;
  #b;
  constructor(a, b) {
    this.#a = a;
    this.#b = b;
  }
  #sum() {
    return #a + #b;
  }
  printSum() {
    console.log(this.#sum());
  }
}
```

#### new.target

用在构造函数中或class中，表示当前new作用的构造函数

```
function Person(name) {
  if (new.target === Person) {
    this.name = name;
  } else {
    throw new Error('必须使用 new 命令生成实例');
  }
}

var person = new Person('张三'); // 正确
var notAPerson = Person.call(person, '张三');  // 报错
```

```
class Shape {
  constructor() {
    if (new.target === Shape) {
      throw new Error('本类不能实例化');
    }
  }
}

class Rectangle extends Shape {
  constructor(length, width) {
    super();
    // ...
  }
}

var x = new Shape();  // 报错
var y = new Rectangle(3, 4);  // 正确
```



