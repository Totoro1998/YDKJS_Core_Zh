##### 1. Each File is a Program [You-Dont-Know-JS/ch2.md at 2nd-ed · getify/You-Dont-Know-JS · GitHub](https://github.com/getify/You-Dont-Know-JS/blob/2nd-ed/get-started/ch2.md#each-file-is-a-program)

    几乎你使用的每个网站(web应用程序)都由许多不同的JS文件组成(通常是. JS文件扩展名)。很容易把整个程序(应用程序)看作一个程序。但JS不同

    在JS中，每个独立文件都是它自己独立的程序

    这个问题的原因主要是关于错误处理。由于JS将文件视为程序，一个文件可能会失败(在解析/编译或执行期间)，这并不一定会阻止处理下一个文件。显然，如果您的应用程序依赖于5个.js文件，并且其中一个失败了，那么在最好情况下，整个应用程序可能只能部分运行。重要的是要确保每个文件都能正常工作，并且在任何可能的范围内，它们都要尽可能优雅地处理其他文件中的故障

    把单独的. JS文件看作单独的JS程序可能会让你感到惊讶。从应用程序的使用角度来看，它确实像是一个大程序。这是因为应用程序的执行允许这些单独的程序相互协作并作为一个程序运行。

| 注意：                                                                 |
|:------------------------------------------------------------------- |
| 许多项目使用构建过程工具，最终将项目中的独立文件组合成一个单独的文件交付到web页面。当这种情况发生时，JS将这个组合文件视为整个程序 |

    多个独立的.js文件作为单个程序的唯一方式是通过“全局作用域”共享它们的状态(以及对它们公共功能的访问)。它们在这个全局作用域名称空间中混合在一起，因此在运行时它们作为整体运行。

    从ES6开始，除了典型的独立JS程序格式外，JS还支持模块格式。模块也是基于文件的。如果通过import语句或<script type=module>标记等模块加载机制加载文件，则其所有代码将被视为单个模块。

    虽然你通常不会把一个模块(一组状态和公开的方法，用于操作该状态)看作一个独立的程序，但JS实际上仍然分开对待每个模块。与“全局作用域”允许独立文件在运行时混合在一起类似，将一个模块导入另一个模块允许它们之间的运行时互操作。

    无论文件(独立的或模块的)使用哪种代码组织模式(和加载机制)，您仍然应该将每个文件视为它自己的(小)程序，然后可以与其他(小)程序协作来执行整个应用程序的功能。

##### 2. Declaring and Using Variables [You-Dont-Know-JS/ch2.md at 2nd-ed · getify/You-Dont-Know-JS · GitHub](https://github.com/getify/You-Dont-Know-JS/blob/2nd-ed/get-started/ch2.md#declaring-and-using-variables)

    let/const/var/function/function的命名参数/catch      声明变量的语法

    const声明的变量不是“unchangeable”，它们只是不能re-assigned。在对象值中使用const是不明智的，因为即使变量不能被重新赋值，这些值仍然可以被更改。

##### 3. Functions [You-Dont-Know-JS/ch2.md at 2nd-ed · getify/You-Dont-Know-JS · GitHub](https://github.com/getify/You-Dont-Know-JS/blob/2nd-ed/get-started/ch2.md#functions)

```
function awesomeFunction(coolThings) {
    // ..
    return amazingStuff;
}        
```

    This is called a function declaration(函数声明) because it appears as a statement by itself, not as an expression in another statement. The association between the identifier `awesomeFunction` and the function value happens during the compile phase of the code, before that code is executed

```
    // let awesomeFunction = ..
// const awesomeFunction = ..
var awesomeFunction = function(coolThings) {
    // ..
    return amazingStuff;
};
```

    This function is an expression that is assigned to the variable `awesomeFunction`. Different from the function declaration form, a function expression is not associated with its identifier until that statement during runtime(运行时)

##### 4. Comparisons [You-Dont-Know-JS/ch2.md at 2nd-ed · getify/You-Dont-Know-JS · GitHub](https://github.com/getify/You-Dont-Know-JS/blob/2nd-ed/get-started/ch2.md#comparisons)

     **All** value comparisons in JS consider the type of the values being compared, not *just* the `===` operator. Specifically, `===` disallows any sort of type conversion (aka, "coercion") in its comparison, where other JS comparisons *do* allow coercion

    "==="操作不允许强制类型转换，其它的JS comparisons允许

```
NaN === NaN;            // false
0 === -0;               // true
```

    Since the lying about such comparisons can be bothersome, it's best to avoid using === for them. For NaN comparisons, use the Number.isNaN(..) utility, which does not lie. For -0 comparison, use the Object.is(..) utility, which also does not lie. Object.is(..) can also be used for non-lying NaN checks, if you prefer. Humorously, you could think of Object.is(..) as the "quadruple-equals" ====, the really-really-strict comparison!（对于上述情况可以使用 Object.is()）严格意义上来说"==="is not actually *strictly exactly equal* comparison

    "=="操作符allows coercion before the comparison. In other words, they both want to compare values of like types, but `==` allows type conversions *first*.注意"=="的本质-它更喜欢简单的数值比较,字符和布尔类型会转换为数值类型

    "==",">","<",">=","<=" use numeric comparisons, except in the case where both values being compared are already strings; 在这种情况下它们使用字符串的字母(类似字典)比较

##### 5.How We Organize in JS [You-Dont-Know-JS/ch2.md at 2nd-ed · getify/You-Dont-Know-JS · GitHub](https://github.com/getify/You-Dont-Know-JS/blob/2nd-ed/get-started/ch2.md#how-we-organize-in-js)

    Two major patterns for organizing code (数据和行为) are used broadly across the JS ecosystem: classes and modules

    类机制允许数据与它们的行为组织在一起，程序也可以在没有任何类定义的情况下构建，但是它的组织可能会更松散，难以阅读和推理，而且更容易出现bug和难以维护

    ES6在原生JS语法中添加了一个模块语法形式，我们稍后会看到。但是在早期的JS中，模块是一个重要且常见的模式，在无数JS程序中都有使用，即使没有专门的语法。

   **Classic Modules**

    classic module 的关键特征是outer function(至少运行一次)，它返回模块的一个“实例”，其中包含一个或多个公开的函数，可以对模块实例的内部(隐藏)的数据进行操作。

    因为这种形式的模块只是一个函数，调用它会产生一个模块的“实例”，所以对这些函数的另一个描述是“模块工厂”。

    **ES Modules**

    您不需要“实例化”一个ES模块，您只需导入它以使用它的单个实例。实际上，esm是“单例”，因为在程序中首次导入时，只创建了一个实例，而其他所有导入都只接收到对同一单例的引用。如果您的模块需要支持多个实例化，那么您必须在ESM定义中提供一个经典的模块样式的工厂函数。
