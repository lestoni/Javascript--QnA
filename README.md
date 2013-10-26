Javascript--QnA
===============

01. When might type coercion occur? How to avoid it? How would you change
a `falsey` or `truthy` into a real boolean?

* Type coercion specifically occurs when using the `==` operator.
*  Use the `===` oprator to avoid type coercion.
* To convert a `falsey` or `truthy` value into boolean , explicitly wrap 
* it using `Boolean()` as in `Boolean('foo')` or idiomatically prefix it with `!!` as in `!!'foo'`.

02. Describe how variable scope works. Explain how to create a closure,
    using a self-executing function(also called IIFE: immediately-invoked
    function expression).

* Variable scope:
    * scope of a variable is where you define it in your source code;
    where the variables defined in the global scope become global variables 
    and the ones defined inside of a function become local variables inside the local scope of the function.

* A closure is a function that is created inside another function,
    where it holds a reference to the outter function's variable 
        object. For example :

        ```
        function foo(){
            var say = 'Hello';

            return function () {
                console.log(say);
            };
        }

        var hi = foo();
        hi(); // 'Hello'
            
        ```

* Immediately invoked function expressions(IIFE's) are commonly used to encapsulate a block of code: 
        * To avoid namespace collisions with other Javascript libraries. Common as a design pattern.
        * To avoid polluting the global scope.

03. Briefly explain how *prototypal inheritance* is differs from class-based
    *classical inheritance*:

    + *Prototype chaining* is the primary method of inheritance in javascript.
    Where the prototype of one reference type can inherit from another prototype. Such that the newly created object inherit methods and properties from across the prototype chain.

04. Describe how the **Module Pattern** works. Explain how the **revealing module pattern** expands upon it:
    
    + The **module pattern** works in the principle of information hiding and providing a public interface. Javascript itself has no concept of explicit private or public members. With this pattern, all private members are defined inside an *Immediately invoked function expression* such that there is no access whatsoever and finally returning a interface which the users can interact with. Example:

        ```
        var app = (function () {
            // private variables
            var _say = 'Hello';

            //private methods
            var sayHi = function () {
                return _say;
            };

            //public interface
            return{
                sayHi: sayHi
            };
        });

        ```
    + The **Revealing pattern** on the hand still works on the same principle of module pattern but it's public interface is more verbose such that instead of returning the method's and properties as defined in module,
    you return them in a style that communicates the purpose instead of the implementation. Example:

        ```
        var app = (function () {
            // private variables
            var _say = 'Hello';

            //private methods
            var sayHi = function () {
                return _say;
            };

            //public interface
            return{
                hi: sayHi // the returned method has a clear name that 
                          //  communicates purpose 
            };
        });

        ```
4. How does a client-side *MVC* or (*MVVM*) approach work? What is you preferred *MV\** framework?
    + Coming soon.

5. Why do these yield different results:
    
    ```
    "1" + 2 + 3; // Equals "123"
    3 + 2 + "1"; // Equals "51"
    3 + 2 + 1; // Equals 6

    ```
    + Javascript is very flexible when it comes to type conversions. Also it is important to understand that javascript gives `+` operator string concatenation the first priority. Plus, most of the javascript operators have a left-to-right associativity .Therefore for the examples above, Javascript resolves the statement as follows:

    ```
        ("1" + 2) + 3; // 2 is type casted to string equivalent of "2" and 
                        // concatenated with "1"
        "12" + 3; // 3 is equivalently type casted to it's string value "3"
                    // and concatenated with "123"
        "123";


        (3 + 2) + "1"; // Normal arithmetic addition to get 5
        5 + "1";    // Type cast 5 to it's string equivalent of "5" and 
                    //concatenated with "1"
        "51";

        3 + 2 + 1; // Normal arithmetic addition to get 5
        5 + 1;      // Normal arithmetic addition to get 6
        6;
        
    ```
6. Why is `0.3` not the result of the following addition? How do you work around this bug:
    
    ```
    0.1 + 0.2;  // Equals to 0.30000000000000004
    ```

    + Note: Numbers in javascript are represented by 64-bit floating-point format.
    In javascript there is problem  of rounding errors in such as trying to perform some mathematical arithmetics for specific floating point values  like above results to such kind of errors. A workaroun would to use less specific values like for example:

    ```
    0.15 + 0.15;  // Equals 0.3
    0.05 + 0.25;  // Also returns 0.3
    ```
7. Describe how variable hoisting works, and how to account for it:

    + When trying to read from variables that have been declared much later inside a function, javascript does what is called **variable hoisting**.

    + In which case , the variable is pulled to the top of the function and as if it is undeclared. Take a look a this example:

    ```
    // How the function declaration appears normally
    function sayHi(){
        // results to undefined
        console.log(hi);

        var hi = 'Hello';

        // results to 'Hello'
        console.log(hi);
    }

    sayHi(); 
    
    // How the interpreter resolves 
    function sayHi(){
        var hi;

        // In which case the result is undefined
        console.log(hi);

        hi = 'Hello';

        // voila! results to 'Hello'
        console.log(hi)
    }

    sayHi();
    ```
    + As a good practice it is always important you define all you variables that are going to be used in the function on the inside top of the function. For example:

    ```
    function getSum(){
        var y;
        var x;

        x = 10;
        y = 20;

        console.log(x + y);
    }

    sayHi(); // results to 30

    ```
8. How do the following defer:

    ```
    function foo() {}

    // versus

    var foo = function () {};
    ```
    + The first example is called  a *function declaration* since it uses the function declaration expression to define it while the second one is called a *function expression* as it uses a function expression expression to define.

    + The two function definitions also differ when it comes to **function hoisting**. Whenever you define a function declaration, you can invoke it after defining it but for function expressions is quite the opposite.Normally, during runtime all the function declarations definitions are pulled or hoisted to the top by the interpreter. Consider the following example:

    ```
    sayHi(); // results to 'Hello'

    sayBye(); // results to TypeError: undefined is not a function

    function sayHi(){
        console.log('Hello');
    }

    var sayBye = function (){
        console.log('Goodbye');
    };

    ```
9. When would you use `*.call()`? - when would you use `*.apply()`?:
    
    + Both methods are used to invoke a function in a given context;

    + `*.call()` method would be quiet useful in the case where arguments needed can be passed just as they are passed in to a normal function. For example:

    ```
    *.call(context, arg1, arg2,...);
    ```

    + On the other hand, `*.apply()` is useful if arguments to be passed are in an array form. For example:

    ```
    *.apply(context, [arg1, arg2, ...]);
    ```
10. How to check if a variable is an Array?
    
    + The Array type has a method called isArray() , that returns true if the given argument is an array type otherwise false. For example:

    ```
    var num = 4;
    var colors = ['red','blue'];

    Array.isArray(num); // returns false

    Array.isArray(colors); // returns true

    ```
11. In the following example, what is foo aliased to? (Hint: It is what `this` 
    means):

    ```
    (function(foo){
        // what is 'foo' aliased to?
    })(this);
    ```
    + 'foo' is aliased to the `window` object since the `this` keyword is resolved to the global object as the IIFE("immediately invoked function expression") is defined in the global namespace.

12. In javascript (and the DOM), variables we consider universal are actually mutable: window, document, undefined. How would you write code to ensure these were predictably available for use? Meaning, assuming someone has injected this code, how would work around it? (Hint: See previous question):
    + Tricky one
13. In one line of code, how you would clone (make a copy of) an array?
    
    ```
    [1,2,2,3].map(function(val){
        return val;
    });
    ```
14. What is the difference between `setInterval` and `setTimeout`? - Bonus: What is the lowest cross-browser increment that each can accurately use?
    
    + Both are javascript timers.
    + `setInterval` is used to execute a block of code passed in as the first argument in a set of time intervals repeatedly. It returns a unique ID used to identify it so that it can be specifically used in `clearInterval`.
    + Meanwhile `setTimeout` executes a block of code after a given period of time. As with `setInterval`, it also returns a unique ID that can be used in `clearTimeout`

15. Explain how `delete` works. What typed of things cannot be deleted?
    
    + The `delete` operator is used to explicitly delete an objects property. For example:

    ```
    var book = {
        genre: 'crime-drama',
        author: 'somebody'
    };

    book.author; // returns 'somebody'

    delete book.author;

    book.author; // returns undefined

    ```

    + This operator cannot not be used to delete:
        + host objects properties. Useragent defined objects properties.
        + Native object properties.
        + User defined object properties that have a data descriptor of `configurable: false`

16. Explain how event delegation works, and when you should use it to handle UI interaction:
    
    + Event delegation is technique used to take advantage of event bubbling:
    where one event handler is defined to handle events of the same type and then it is attached to the highest node possible in the document tree.

    + It is highly fruitful to use this technique in cases where there is heavy event handling in the UI plus it has a very positive impact on performance and less memory usage.

17. What does this snippet of code do?
    
    ```
    var foo = bar ? bar : 0;
    ```

    + The example uses a ternary operator whereby if the **bar** is a `truthy` value then it is set otherwise if it a `falsey` then **0** is set.

18. When might you write something like this, and what is it shorthand for?
    
    ```
    foo && foo.bar();
    ```
    
    + The logical AND (`&&`) operator is quite useful when the condition to be passed into a loop is required to return  `true` for both left and right operands otherwise evaluates to `false`.

    + In the example above, it evaluates as follows:

        + If **foo** is a `truthy` , evaluate the second operand otherwise just return `false` for the condition.
        + If the value return by **foo.bar()** is a `truthy` evaluate the whole condition as `true` otherwise evaluate as `false`.

    + The example is a shorthand for:

        ```
        if(foo){
            foo.bar();
        }
        ```
19. How do `parseInt` and `parseFloat` differ? When would you use `*.toFixed` ? - In which instance might the following code snippet actually make sense to use?
    
    ```
    var my_number= my_string - 0;
    ```

    + `parseInt` method returns an integer number equivalent of the passed argument otherwise it returns `NaN`. The same goes with `parseFloat` where it returns the floating-point number equivalent of the argument passed otherwise returns 'NaN';

    + `parseInt` takes an optional argument ,*radix*, of the number to be passed.

    ```
    var num = 123;
    var num2 = '123.456 digits';
    var str = 'string';
    var str2 = 'string 10'

    parseInt(num);  // returns 123
    parseInt(num, 16); // returns 291 as the Hexadecimal equivalent 
    parseInt(num2); // returns 123
    parseInt(str);  // returns NaN
    parseInt(str2);  // returns NaN

    parseFloat(num);  // returns 123
    parseFloat(num2); // returns 123.456
    parseFloat(str);  // returns NaN
    parseFloat(str2);  // returns NaN

    ```

    + `*.toFixed` method can come in handy when need arises to get number with a fixed decimal values . For Example:

    ```
    var num = 12.3445555;

    num.toFixed(2); // returns 12.34
    num.toFixed();  // returns 12
    num.toFixed(6); // returns 12.344556

    ```

    + The first example would be clear , as the variable name suggests, if the `my_string` is a number type or passed in either `parseInt` or `parseFloat` method to get the number equivalent.
