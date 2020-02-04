# class继承

#### 使用extends关键字

- 其中super表示父类构造函数
- 子类constructor中必须调用super塑造this，之后才能使用this

**es5与es6继承不同之处**

ES5 的继承，实质是先创造子类的实例对象`this`，然后再将父类的方法添加到`this`上面（`Parent.apply(this)`）。ES6 的继承机制完全不同，实质是先将父类实例对象的属性和方法，加到`this`上面（所以必须先调用`super`方法），然后再用子类的构造函数修改`this`。

```
class Point {
}

class ColorPoint extends Point {
 constructor(x, y, color) {
    super(x, y); // 调用父类的constructor(x, y)
    this.color = color;
  }

  toString() {
    return this.color + ' ' + super.toString(); // 调用父类的toString()
  }
}
```

- Object.getPrototypeOf()用于从子类上获取父类,可用于判断继承关系

```
Object.getPrototypeOf(ColorPoint) === Point
// true
```

#### super

- 作为函数使用时，只能用在constructor中，表示父类构造函数
- 作为对象使用时

**用在class普通方法中，表示父类原型对象**

**super中this指向子类实例，通过super对某个属性赋值，则是在子类实例上**

````
class A {
  constructor() {
    this.x = 1;
  }
}

class B extends A {
  constructor() {
    super();
    this.x = 2;
    super.x = 3;
    console.log(super.x); // undefined
    console.log(this.x); // 3
  }
}

let b = new B();
````

**用在静态方法中，表示父类**

**静态方法中的this表示子类**

```
class A {
  constructor() {
    this.x = 1;
  }
  static print() {
    console.log(this.x);
  }
}

class B extends A {
  constructor() {
    super();
    this.x = 2;
  }
  static m() {
    super.print();
  }
}

B.x = 3;
B.m() // 3
```

#### 两条继承链

- 子类`__proto__`指向父类，表示构造函数的继承
- 子类的`prototype`的`__proro__`指向父类的prototype，表示方法的继承

```
class A {
}

class B extends A {
}

B.__proto__ === A // true
B.prototype.__proto__ === A.prototype // true
```

#### ES6允许继承原生构造函数定义子类

因为 ES6 是先新建父类的实例对象`this`，然后再用子类的构造函数修饰`this`，使得父类的所有行为都可以继承



#### mixin多对象混入函数

```
function mix(...mixins){
	class Mix {
		constructor(){
			for(let mixin of mixins){
				copyProperties(this, new mixin());//拷贝实例属性
			}
		}
	}
	for(let mixin of mixins){
		copyProperties(Mix, mixin);//拷贝静态属性
		copyProperties(Mix.prototype, mixin.prototype);//拷贝原型属性
	}
}
function copyProperties(target, source){
	for(let key of Reflect.ownKeys(source)){
		if(key !=='constructor'&&key!=='prototype'&&key!=='name'){
		let desc=Object.getOwnPropertyDescriptor(source, key);
		Object.defineProperty(target, key, desc);
		}
	}
}

class DistributedEdit extends mix(Loggable, Serializable) {
  // ...
}
```

