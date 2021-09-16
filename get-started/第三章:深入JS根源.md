## 迭代 [You-Dont-Know-JS/ch3.md at 2nd-ed · getify/You-Dont-Know-JS · GitHub](https://github.com/getify/You-Dont-Know-JS/blob/2nd-ed/get-started/ch3.md#iteration)

    ES6 standardized a specific protocol for the iterator pattern directly in the language. The protocol defines a next() method whose return is an object called an iterator result; the object has value and done properties, where done is a boolean that is false until the iteration over the underlying data source is complete

#### 使用迭代器 [You-Dont-Know-JS/ch3.md at 2nd-ed · getify/You-Dont-Know-JS · GitHub](https://github.com/getify/You-Dont-Know-JS/blob/2nd-ed/get-started/ch3.md#consuming-iterators)

    "for...of "," ..."

#### 可迭代的 [You-Dont-Know-JS/ch3.md at 2nd-ed · getify/You-Dont-Know-JS · GitHub](https://github.com/getify/You-Dont-Know-JS/blob/2nd-ed/get-started/ch3.md#iterables)

    ES6 defined the basic data structure/collection types in JS as iterables. This includes strings, arrays, maps, sets, and others.

    ES6在JS中将基本数据结构/集合类型定义为可迭代对象。这包括字符串、数组、映射、集合等。

    Each of the built-in iterables in JS expose a default iteration, one which likely matches your intuition. But you can also choose a more specific iteration if necessary

    *The iteration-consumption protocol expects an iterable, but the reason we can provide a direct iterator is that an iterator is just an iterable of itself! When creating an iterator instance from an existing iterator, the iterator itself is returned.* （什么意思）

## 闭包

    Closure is when a function remembers and continues to access variables from outside its scope, even when the function is executed in a different scope

    闭包是指函数记住并继续从其作用域之外访问变量，即使函数是在不同的作用域执行的

    to observe a closure, you must execute a function in a different scope than where that function was originally defined

```
function greeting(msg) {
    return function who(name) {
        console.log(`${ msg }, ${ name }!`);
    };
}

var hello = greeting("Hello");
var howdy = greeting("Howdy");

hello("Kyle");
// Hello, Kyle!

hello("Sarah");
// Hello, Sarah!

howdy("Grant");
// Howdy, Grant!
```

    When the `greeting(..)` function finishes running, normally we would expect all of its variables to be garbage collected (removed from memory). We'd expect each `msg` to go away, but they don't. The reason is closure. Since the inner function instances are still alive (assigned to `hello` and `howdy`, respectively), their closures are still preserving the `msg` variables.

    These closures are not a snapshot of the `msg` variable's value; they are a direct link and preservation of the variable itself. That means closure can actually observe (or make!) updates to these variables over time

    Closure is most common when working with asynchronous code, such as with callbacks

    It's not necessary that the outer scope be a function—it usually is, but not always—just that there be at least one variable in an outer scope accessed from an inner function （外部作用域不一定是函数(通常是，但不总是如此)，只是外部作用域中至少有一个变量可以从内部函数访问）

```
    
```

## this关键字 https://github.com/getify/You-Dont-Know-JS/blob/2nd-ed/get-started/ch3.md#this-keyword

    One common misconception is that a function's `this` refers to the function itself. Because of how `this` works in other languages, another misconception is that `this` points the instance that a method belongs to. **Both are incorrect**

    当一个函数被定义后，它通过闭包被附加到它的外围作用域，作用域是控制如何解析对变量引用的一组规则

    But functions also have another characteristic besides their scope that influences what they can access. This characteristic is best described as an execution context, and it's exposed to the function via its this keyword.

    但除了作用域之外，函数还有另一个特性，这影响了它们可以访问的内容。这个特性描述为一个执行上下文最好，它通过其this关键字暴露给函数（this指执行上下文，影响着可以访问的内容）

    Scope is static and contains a fixed set of variables available at the moment and location you define a function, but a function's execution context is dynamic, entirely dependent on how it is called (regardless of where it is defined or even called from)

    作用域是静态的，**包含一组固定的可用变量和定义函数时的位置**，**但函数的执行上下文是动态的**，完全取决于它是如何被调用的(不管它是在哪里定义的，甚至是从哪里调用的)。

    `this` is not a fixed characteristic of a function based on the function's definition, but rather a dynamic characteristic that's determined each time the function is called（每次函数调用时的动态特征）

    One way to think about the execution context is that it's a tangible object whose properties are made available to a function while it executes. Compare that to scope, which can also be thought of as an object; except, the scope object is hidden inside the JS engine, it's always the same for that function, and its properties take the form of identifier variables available inside the function

    想象执行上下文的一种方法是，它是一个有形的对象，它的属性在执行时对函数可用。将其与scope相比较，scope也可以被认为是一个对象;但是，scope对象隐藏在JS引擎中，对于该函数来说，它总是相同的，它的属性采用函数中可用的标识符变量的形式

```
var homework = {
    topic: "JS",
    assignment: assignment
};

homework.assignment();
```

    A copy of the `assignment` function reference is set as a property on the `homework` object, and then it's called as `homework.assignment()`. That means the `this` for that function call will be the `homework` object.

## 原型 https://github.com/getify/You-Dont-Know-JS/blob/2nd-ed/get-started/ch3.md#prototypes

    prototype是一个对象的特征，是property获取的解决方式。

    Think about a prototype as a linkage between two objects; the linkage is hidden behind the scenes, though there are ways to expose and observe it. This prototype linkage occurs when an object is created; it's linked to another object that already exists

    A series of objects linked together via prototypes is called the "prototype chain."

    The purpose of this prototype linkage (i.e., from an object B to another object A) is so that 访问B没有的properties/methods, are delegated to A to handle. Delegation（委托）of property/method access allows two (or more!) objects to cooperate with each other to perform a task.

```
    var homework = {
    topic: "JS"
};
```

    The `homework` object only has a single property on it: `topic`. However, its default prototype linkage connects to the `Object.prototype` object, which has common built-in methods on it like `toString()` and `valueOf()`,

    `homework.toString()` works even though `homework` doesn't have a `toString()` method defined; the delegation invokes `Object.prototype.toString()` instead

#### Object Linkage

    要定义一个对象原型链接，你可以使用**object .create**(..)工具来创建对象:

    The first argument to Object.create(..) specifies（指定了） an object to link the newly created object to, and then returns the newly created (and linked!) object.

    Delegation through the prototype chain only applies for accesses to lookup（查找） the value in a property. If you assign（赋值） to a property of an object, that will apply directly to the object regardless of where that object is prototype linked to (本质上prototype还是为了查找，不是为了赋值)

    Object.create(null) creates an object that is not prototype linked anywhere, so it's purely just a standalone（独立的） object; in some circumstances（情况下）, that may be preferable

```
var homework = {
    topic: "JS"
};
var otherHomework = Object.create(homework);
otherHomework.topic;   // "JS"
otherHomework.topic = "Math"; 找到该值后直接赋值了，不需要根据原型链往上找homeworkd对象
otherHomework.topic;
// "Math"
homework.topic;
// "JS" -- not "Math"

https://github.com/getify/You-Dont-Know-JS/blob/2nd-ed/get-started/images/fig5.png
```

    We covered the this keyword earlier, but its true importance shines when considering how it powers prototype-delegated function calls. Indeed, one of the main reasons this supports dynamic context based on how the function is called is so that method calls on objects which delegate through the prototype chain still maintain the expected this (**什么意思**)

    我们在前面介绍了this关键字，但是当考虑它如何支持原型委托函数调用时，它的真正重要性就会凸显出来。实际上，它支持基于函数调用方式的动态上下文的主要原因之一是，通过原型链委托的对象的方法调用仍然保持预期的this

```
    var homework = {
    study() {
        console.log(`Please study ${ this.topic }`);
    }
};

var jsHomework = Object.create(homework);
jsHomework.topic = "JS";
jsHomework.study();
// Please study JS

var mathHomework = Object.create(homework);
mathHomework.topic = "Math";
mathHomework.study();
```

    The two objects `jsHomework` and `mathHomework` each prototype link to the single `homework` object, which has the `study()` function. `jsHomework` and `mathHomework` are each given their own `topic` property ![(see Figure 6).](/Users/totoro.fu/Desktop/fig6.png)

    jsHomework.study() delegates to homework.study(), but its this (this.topic) for that execution resolves to jsHomework because of how the function is called, so this.topic is "JS".
