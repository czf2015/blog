# static

类(class)通过static关键字定义静态方法。不能在类的实例上调用静态方法，而应该通过类本身调用。静态方法通常用于创建实用程序函数。

静态方法调用类中的其他静态方法，可使用this关键字。非静态方法中，不能直接使用this关键子来访问静态方法。而是要通过类名来调用：CLASSNAME.STATIC_METHOD_NAME()，或者用构造函数的属性来调用该方法：this.constructor.STATIC_METHOD_NAME().