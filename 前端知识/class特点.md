# class特点

- class声明提升，会有暂时性死区，与let,const同
- 内部使用严格模式
- class内的方法不可枚举
- class中的方法没有原型也没有construct，不能通过new 调用
- 必须使用new来调用class
- 子类可以通过proto寻址到父类
- this先继承父类实例，然后再子类构造函数来修饰

而es5中，this先继承子类实例，再父类构造函数修饰