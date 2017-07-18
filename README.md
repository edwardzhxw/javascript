# JavaScript Style Guide

## <a name="table-of-contents">目录</a>
  1. [对象](#objects)
  2. [数组](#arrays)
  3. [字符串](#strings)
  4. [函数](#functions)
  5. [属性](#properties)
  6. [比较运算符 & 等号](#comparison-operators--equality)
  7. [块](#blocks)
  8. [控制语句](#control-statements)
  9. [注释](#comments)
  10. [空格](#whitespace)
  11. [逗号](#commas)
  12. [分号](#semicolons)
  13. [类型转化](#type-casting--coercion)
  14. [命名规则](#naming-conventions)

## 对象

  <a name="objects"></a>
  <a name="objects--no-new"></a><a name="1.1"></a>
  - [1.1](#objects--no-new) 使用直接量创建对象。 eslint: [`no-new-object`](http://eslint.org/docs/rules/no-new-object.html)

    ```javascript
    // bad
    var item = new Object();

    // good
    var item = {};
    ```

  <a name="objects--quoted-props"></a><a name="1.2"></a>
  - [1.2](#objects--quoted-props) 避免使用无意义的引号。 eslint: [`quote-props`](http://eslint.org/docs/rules/quote-props.html) jscs: [`disallowQuotedKeysInObjects`](http://jscs.info/rule/disallowQuotedKeysInObjects)

    ```javascript
    // bad
    var bad = {
      'foo': 3,
      'bar': 4,
      'data-blah': 5,
    };

    // good
    var good = {
      foo: 3,
      bar: 4,
      'data-blah': 5,
    };
    ```

  <a name="objects--prototype-builtins"><a name="1.3"></a>
  - [1.3](#objects--prototype-builtins) 不要直接调用`Object.prototype`的方法, 比如`hasOwnProperty`, `propertyIsEnumerable`和`isPrototypeOf`。

    ```javascript
    // bad
    console.log(object.hasOwnProperty(key));

    // good
    console.log(Object.prototype.hasOwnProperty.call(object, key));

    // best
    const has = Object.prototype.hasOwnProperty; // cache the lookup once, in module scope.
    console.log(has.call(object, key));
    ```

**[⬆ back to top](#table-of-contents)**

## 数组

  <a name="arrays"></a>
  <a name="arrays--literals"></a><a name="2.1"></a>
  - [2.1](#arrays--literals) 使用直接量创建数组。 eslint: [`no-array-constructor`](http://eslint.org/docs/rules/no-array-constructor.html)

    ```javascript
    // bad
    var items = new Array();

    // good
    var items = [];
    ```

  <a name="arrays--push"></a><a name="2.2"></a>
  - [2.2](#arrays--push) 使用 [Array#push](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/push) 向数组增加元素时使用 Array#push 来替代直接赋值。

    ```javascript
    var someStack = [];

    // bad
    someStack[someStack.length] = 'abracadabra';

    // good
    someStack.push('abracadabra');
    ```

  <a name="arrays--callback-return"></a><a name="2.3"></a>
  - [2.3](#arrays--callback-return) Use return statements in array method callbacks. It’s ok to omit the return if the function body consists of a single statement returning an expression without side effects, following [8.2](#arrows--implicit-return). eslint: [`array-callback-return`](http://eslint.org/docs/rules/array-callback-return)

    ```javascript
    // good
    [1, 2, 3].map((x) => {
      var y = x + 1;
      return x * y;
    });

    // good
    [1, 2, 3].map(x => x + 1);

    // bad
    const flat = {};
    [[0, 1], [2, 3], [4, 5]].reduce((memo, item, index) => {
      const flatten = memo.concat(item);
      flat[index] = flatten;
    });

    // good
    const flat = {};
    [[0, 1], [2, 3], [4, 5]].reduce((memo, item, index) => {
      const flatten = memo.concat(item);
      flat[index] = flatten;
      return flatten;
    });

    // bad
    inbox.filter((msg) => {
      const { subject, author } = msg;
      if (subject === 'Mockingbird') {
        return author === 'Harper Lee';
      } else {
        return false;
      }
    });

    // good
    inbox.filter((msg) => {
      const { subject, author } = msg;
      if (subject === 'Mockingbird') {
        return author === 'Harper Lee';
      }

      return false;
    });
    ```

<a name="arrays--bracket-newline"><a name="2.4"></a>
  - [2.4](#arrays--bracket-newline) Use line breaks after open and before close array brackets if an array has multiple lines

  ```javascript
  // bad
  var arr = [
    [0, 1], [2, 3], [4, 5],
  ];

  var objectInArray = [{
    id: 1,
  }, {
    id: 2,
  }];

  // good
  var arr = [[0, 1], [2, 3], [4, 5]];

  var objectInArray = [
    {
      id: 1,
    },
    {
      id: 2,
    },
  ];
  ```

**[⬆ back to top](#table-of-contents)**

## 字符串

  <a name="strings"></a>
  <a name="strings--quotes"></a><a name="3.1"></a>
  - [3.1](#strings--quotes) 字符串使用单引号`''`。 eslint: [`quotes`](http://eslint.org/docs/rules/quotes.html) jscs: [`validateQuoteMarks`](http://jscs.info/rule/validateQuoteMarks)

    ```javascript
    // bad
    var name = "Capt. Janeway";

    // good
    var name = 'Capt. Janeway';
    ```

  <a name="strings--line-length"></a><a name="3.2"></a>
  - [3.2](#strings--line-length) 当字符串过长时，不要将字符串拆成多行。

    > 多行的字符串不利于阅读，也不利于搜索。

    ```javascript
    // bad
    var errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

    // bad
    var errorMessage = 'This is a super long error that was thrown because ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.';

    // good
    var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';
    ```

  <a name="strings--eval"></a><a name="3.3"></a>
  - [3.3](#strings--eval) 字符串不要使用`eval()`, 容易引起安全隐患。 eslint: [`no-eval`](http://eslint.org/docs/rules/no-eval)

  <a name="strings--escaping"><a name="3.4"></a>
  - [3.4](#strings--escaping) 不要做不必要的字符串转义。 eslint: [`no-useless-escape`](http://eslint.org/docs/rules/no-useless-escape)

    ```javascript
    // bad
    var foo = '\'this\' \i\s \"quoted\"';

    // good
    var foo = '\'this\' is "quoted"';
    ```

**[⬆ back to top](#table-of-contents)**

## 函数

  <a name="functions"></a>
  <a name="functions--declarations"></a><a name="4.1"></a>
  - [4.1](#functions--declarations) Use named function expressions instead of function declarations. eslint: [`func-style`](http://eslint.org/docs/rules/func-style) jscs: [`disallowFunctionDeclarations`](http://jscs.info/rule/disallowFunctionDeclarations)

    > Why? Function declarations are hoisted, which means that it’s easy - too easy - to reference the function before it is defined in the file. This harms readability and maintainability. If you find that a function’s definition is large or complex enough that it is interfering with understanding the rest of the file, then perhaps it’s time to extract it to its own module! Don’t forget to name the expression - anonymous functions can make it harder to locate the problem in an Error’s call stack. ([Discussion](https://github.com/airbnb/javascript/issues/794))

    ```javascript
    // bad
    function foo() {
      // ...
    }

    // bad
    var foo = function () {
      // ...
    };

    // good
    var foo = function bar() {
      // ...
    };
    ```

  <a name="functions--iife"></a><a name="4.2"></a>
  - [4.2](#functions--iife) Wrap immediately invoked function expressions in parentheses. eslint: [`wrap-iife`](http://eslint.org/docs/rules/wrap-iife.html) jscs: [`requireParenthesesAroundIIFE`](http://jscs.info/rule/requireParenthesesAroundIIFE)

    > Why? An immediately invoked function expression is a single unit - wrapping both it, and its invocation parens, in parens, cleanly expresses this. Note that in a world with modules everywhere, you almost never need an IIFE.

    ```javascript
    // immediately-invoked function expression (IIFE)
    (function () {
      console.log('Welcome to the Internet. Please follow me.');
    }());
    ```

  <a name="functions--in-blocks"></a><a name="4.3"></a>
  - [4.3](#functions--in-blocks) 不要在非函数的块中定义函数(`if`, `while`, 等等), 把函数分配给一个变量。 eslint: [`no-loop-func`](http://eslint.org/docs/rules/no-loop-func.html)

    ```javascript
    // bad
    if (currentUser) {
      function test() {
        console.log('Nope.');
      }
    }

    // good
    var test;
    if (currentUser) {
      test = function() {
        console.log('Yup.');
      };
    }
    ```

  <a name="functions--arguments-shadow"></a><a name="4.5"></a>
  - [4.5](#functions--arguments-shadow) 不要使用`arguments`参数在函数定义中。

    ```javascript
    // bad
    function foo(name, options, arguments) {
      // ...
    }

    // good
    function foo(name, options, args) {
      // ...
    }
    ```

  <a name="functions--constructor"></a><a name="4.6"></a>
  - [4.6](#functions--constructor) 不要使用构造函数创建一个新函数。 eslint: [`no-new-func`](http://eslint.org/docs/rules/no-new-func)

    ```javascript
    // bad
    var add = new Function('a', 'b', 'return a + b');

    // still bad
    var subtract = Function('a', 'b', 'return a - b');
    ```

  <a name="functions--signature-spacing"></a><a name="4.7"></a>
  - [4.7](#functions--signature-spacing) 函数定义中的空格 eslint: [`space-before-function-paren`](http://eslint.org/docs/rules/space-before-function-paren) [`space-before-blocks`](http://eslint.org/docs/rules/space-before-blocks)

    ```javascript
    // bad
    var f = function(){};
    var g = function (){};
    var h = function() {};

    // good
    var x = function () {};
    var y = function a() {};
    ```

  <a name="functions--signature-invocation-indentation"></a><a name="4.8"></a>
  - [4.8](#functions--signature-invocation-indentation) 函数定义时多行参数时，每一个参数为一行，并且在末尾添加逗号。

    ```javascript
    // bad
    function foo(bar,
                 baz,
                 quux) {
      // ...
    }

    // good
    function foo(
      bar,
      baz,
      quux,
    ) {
      // ...
    }

    // bad
    console.log(foo,
      bar,
      baz);

    // good
    console.log(
      foo,
      bar,
      baz,
    );
    ```

**[⬆ back to top](#table-of-contents)**

## 属性

  <a name="properties"></a>
  <a name="properties--dot"></a><a name="5.1"></a><a name="5.1"></a>
  - [5.1](#properties--dot) 使用逗号来访问属性。 eslint: [`dot-notation`](http://eslint.org/docs/rules/dot-notation.html) jscs: [`requireDotNotation`](http://jscs.info/rule/requireDotNotation)

    ```javascript
    var luke = {
      jedi: true,
      age: 28,
    };

    // bad
    var isJedi = luke['jedi'];

    // good
    var isJedi = luke.jedi;
    ```

**[⬆ back to top](#table-of-contents)**

## 比较运算符 & 等号

  <a name="comparison-operators--equality"></a>
  <a name="comparison--eqeqeq"></a><a name="6.1"></a>
  - [6.1](#comparison--eqeqeq) 优先使用 `===` 和 `!==`, 而不是 `==` 和 `!=`. eslint: [`eqeqeq`](http://eslint.org/docs/rules/eqeqeq.html)

  <a name="comparison--if"></a><a name="6.2"></a>
  - [6.2](#comparison--if) 条件表达式例如 `if` 语句通过抽象方法 `ToBoolean` 强制计算它们的表达式并且总是遵守下面的规则:

    - **对象** 计算为 **true**
    - **Undefined** 计算为 **false**
    - **Null** 计算为 **false**
    - **布尔值** 计算为 **布尔值**
    - **数字** 如果是 **+0, -0, or NaN** 计算为 **false**, 其他则是 **true**
    - **字符串** 如果是空字符串 `''` 计算为 **false**, 其他则是 **true**

  <a name="comparison--shortcuts"></a><a name="6.3"></a>
  - [6.3](#comparison--shortcuts) 使用快捷方式, 除了字符串和数字。

    ```javascript
    // bad
    if (isValid === true) {
      // ...
    }

    // good
    if (isValid) {
      // ...
    }

    // bad
    if (name) {
      // ...
    }

    // good
    if (name !== '') {
      // ...
    }

    // bad
    if (collection.length) {
      // ...
    }

    // good
    if (collection.length > 0) {
      // ...
    }
    ```

  <a name="comparison--switch-blocks"></a><a name="6.4"></a>
  - [6.4](#comparison--switch-blocks) 在 `case` 和 `default` 中使用块。

    eslint rules: [`no-case-declarations`](http://eslint.org/docs/rules/no-case-declarations.html).

    ```javascript
    // bad
    switch (foo) {
      case 1:
        var x = 1;
        break;
      case 2:
        var y = 2;
        break;
      case 3:
        function f() {
          // ...
        }
        break;
      default:
        // ...
    }

    // good
    switch (foo) {
      case 1: {
        var x = 1;
        break;
      }
      case 2: {
        const y = 2;
        break;
      }
      case 3: {
        function f() {
          // ...
        }
        break;
      }
      case 4:
        bar();
        break;
      default: {
        // ...
      }
    }
    ```

  <a name="comparison--nested-ternaries"></a><a name="6.5"></a>
  - [6.5](#comparison--nested-ternaries) 三元运算符? 和 : 与他们所负责的代码处于同一行。

    eslint rules: [`no-nested-ternary`](http://eslint.org/docs/rules/no-nested-ternary.html).

    ```javascript
    // bad
    var foo = maybe1 > maybe2
      ? "bar"
      : value1 > value2 ? "baz" : null;

    // better
    var maybeNull = value1 > value2 ? 'baz' : null;

    var foo = maybe1 > maybe2
      ? 'bar'
      : maybeNull;

    // best
    var maybeNull = value1 > value2 ? 'baz' : null;

    var foo = maybe1 > maybe2 ? 'bar' : maybeNull;
    ```

  <a name="comparison--unneeded-ternary"></a><a name="6.6"></a>
  - [6.6](#comparison--unneeded-ternary) 避免不需要的三元运算符。

    eslint rules: [`no-unneeded-ternary`](http://eslint.org/docs/rules/no-unneeded-ternary.html).

    ```javascript
    // bad
    var foo = a ? a : b;
    var bar = c ? true : false;
    var baz = c ? false : true;

    // good
    var foo = a || b;
    var bar = !!c;
    var baz = !c;
    ```

**[⬆ back to top](#table-of-contents)**

## 块

  <a name="blocks"></a>
  <a name="blocks--braces"></a><a name="7.1"></a>
  - [7.1](#blocks--braces) 使用大括号包裹所有的多行代码块。

    ```javascript
    // bad
    if (test)
      return false;

    // good
    if (test) return false;

    // good
    if (test) {
      return false;
    }

    // bad
    function foo() { return false; }

    // good
    function bar() {
      return false;
    }
    ```

  <a name="blocks--cuddled-elses"></a><a name="7.2"></a>
  - [7.2](#blocks--cuddled-elses) 如果通过 if 和 else 使用多行代码块，把 else 放在 if 代码块关闭括号的同一行。 eslint: [`brace-style`](http://eslint.org/docs/rules/brace-style.html) jscs:  [`disallowNewlineBeforeBlockStatements`](http://jscs.info/rule/disallowNewlineBeforeBlockStatements)

    ```javascript
    // bad
    if (test) {
      thing1();
      thing2();
    }
    else {
      thing3();
    }

    // good
    if (test) {
      thing1();
      thing2();
    } else {
      thing3();
    }
    ```

**[⬆ back to top](#table-of-contents)**

## 控制语句

  <a name="control-statements"></a><a name="8.1"></a>
  - [8.1](#control-statements) 如果控制语句(`if`, `while` 等等)过长，将每一组条件单独放在一行。逻辑运算符在最前或最后并不重要。

    ```javascript
    // bad
    if ((foo === 123 || bar === 'abc') && doesItLookGoodWhenItBecomesThatLong() && isThisReallyHappening()) {
      thing1();
    }

    // bad
    if (foo === 123 &&
      bar === 'abc') {
      thing1();
    }

    // bad
    if (foo === 123
      && bar === 'abc') {
      thing1();
    }

    // good
    if (
      (foo === 123 || bar === "abc") &&
      doesItLookGoodWhenItBecomesThatLong() &&
      isThisReallyHappening()
    ) {
      thing1();
    }

    // good
    if (foo === 123 && bar === 'abc') {
      thing1();
    }

    // good
    if (
      foo === 123 &&
      bar === 'abc'
    ) {
      thing1();
    }

    // good
    if (
      foo === 123
      && bar === 'abc'
    ) {
      thing1();
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Comments

  <a name="comments"></a>
  <a name="comments--multiline"></a><a name="9.1"></a>
  - [9.1](#comments--multiline) 多行注释优先使用 `/** ... */`。

    ```javascript
    // bad
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {

      // ...

      return element;
    }

    // good
    /**
     * make() returns a new element
     * based on the passed-in tag name
     */
    function make(tag) {

      // ...

      return element;
    }
    ```

  <a name="comments--singleline"></a><a name="9.2"></a>
  - [9.2](#comments--singleline) 使用 `//` 作为单行注释。单行注释放在单独一行内。如果注释是块的第一行则不需要空白行，其他需要一个空白行。

    ```javascript
    // bad
    const active = true;  // is current tab

    // good
    // is current tab
    const active = true;

    // bad
    function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      const type = this.type || 'no type';

      return type;
    }

    // good
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      const type = this.type || 'no type';

      return type;
    }

    // also good
    function getType() {
      // set the default type to 'no type'
      const type = this.type || 'no type';

      return type;
    }
    ```

  - (#comments--spaces) 所有注释由空格开始，便于阅读。 eslint: [`spaced-comment`](http://eslint.org/docs/rules/spaced-comment)

    ```javascript
    // bad
    //is current tab
    const active = true;

    // good
    // is current tab
    const active = true;

    // bad
    /**
     *make() returns a new element
     *based on the passed-in tag name
     */
    function make(tag) {

      // ...

      return element;
    }

    // good
    /**
     * make() returns a new element
     * based on the passed-in tag name
     */
    function make(tag) {

      // ...

      return element;
    }
    ```

  <a name="comments--actionitems"></a><a name="9.3"></a>
  - [9.3](#comments--actionitems) 如果在开发中遇到问题，可以利用 `FIXME` 或 `TODO` 记录位置，便于之后做修改。

  <a name="comments--fixme"></a><a name="9.4"></a>
  - [9.4](#comments--fixme)

    ```javascript
    function Calculator() {

      // FIXME: shouldn't use a global here
      total = 0;

      return this;
    }
    ```

  <a name="comments--todo"></a>
  - (#comments--todo)

    ```javascript
    function Calculator() {

      // TODO: total should be configurable by an options param
      this.total = 0;

      return this;
    }
    ```

**[⬆ back to top](#table-of-contents)**

## 空格

  <a name="whitespace"></a><a name="10.1"></a>
  - [10.1](#whitespace--spaces) 两个空格为缩进. eslint: [`indent`](http://eslint.org/docs/rules/indent.html) jscs: [`validateIndentation`](http://jscs.info/rule/validateIndentation)

    ```javascript
    // bad
    function foo() {
    ∙∙∙∙var name;
    }

    // bad
    function bar() {
    ∙var name;
    }

    // good
    function baz() {
    ∙∙var name;
    }
    ```

  <a name="whitespace--before-blocks"></a><a name="10.2"></a>
  - [10.2](#whitespace--before-blocks) 在块前需要一个空格。 eslint: [`space-before-blocks`](http://eslint.org/docs/rules/space-before-blocks.html) jscs: [`requireSpaceBeforeBlockStatements`](http://jscs.info/rule/requireSpaceBeforeBlockStatements)

    ```javascript
    // bad
    function test(){
      console.log('test');
    }

    // good
    function test() {
      console.log('test');
    }

    // bad
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    });

    // good
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    });
    ```

  <a name="whitespace--around-keywords"></a><a name="10.3"></a>
  - [10.3](#whitespace--around-keywords) 在控制语句(`if`, `while` 等等)的小括号前放一个空格。在函数调用及声明中，不在函数的参数列表前加空格。 eslint: [`keyword-spacing`](http://eslint.org/docs/rules/keyword-spacing.html) jscs: [`requireSpaceAfterKeywords`](http://jscs.info/rule/requireSpaceAfterKeywords)

    ```javascript
    // bad
    if(isJedi) {
      fight ();
    }

    // good
    if (isJedi) {
      fight();
    }

    // bad
    function fight () {
      console.log ('Swooosh!');
    }

    // good
    function fight() {
      console.log('Swooosh!');
    }
    ```

  <a name="whitespace--infix-ops"></a><a name="10.4"></a>
  - [10.4](#whitespace--infix-ops) 使用空格把运算符隔开。 eslint: [`space-infix-ops`](http://eslint.org/docs/rules/space-infix-ops.html) jscs: [`requireSpaceBeforeBinaryOperators`](http://jscs.info/rule/requireSpaceBeforeBinaryOperators), [`requireSpaceAfterBinaryOperators`](http://jscs.info/rule/requireSpaceAfterBinaryOperators)

    ```javascript
    // bad
    var x=y+5;

    // good
    var x = y + 5;
    ```

  <a name="whitespace--chains"></a><a name="10.5"></a>
  - [10.5](#whitespace--chains) 在使用长方法链时进行缩进。使用前面的点 . 强调这是方法调用而不是新语句。 eslint: [`newline-per-chained-call`](http://eslint.org/docs/rules/newline-per-chained-call) [`no-whitespace-before-property`](http://eslint.org/docs/rules/no-whitespace-before-property)

    ```javascript
    // bad
    $('#items').find('.selected').highlight().end().find('.open').updateCount();

    // bad
    $('#items').
      find('.selected').
        highlight().
        end().
      find('.open').
        updateCount();

    // good
    $('#items')
      .find('.selected')
        .highlight()
        .end()
      .find('.open')
        .updateCount();

    // bad
    var leds = stage.selectAll('.led').data(data).enter().append('svg:svg').classed('led', true)
        .attr('width', (radius + margin) * 2).append('svg:g')
        .attr('transform', `translate(${radius + margin},${radius + margin})`)
        .call(tron.led);

    // good
    var leds = stage.selectAll('.led')
        .data(data)
      .enter().append('svg:svg')
        .classed('led', true)
        .attr('width', (radius + margin) * 2)
      .append('svg:g')
        .attr('transform', `translate(${radius + margin},${radius + margin})`)
        .call(tron.led);

    // good
    var leds = stage.selectAll('.led').data(data);
    ```

  <a name="whitespace--after-blocks"></a><a name="10.6"></a>
  - [10.6](#whitespace--after-blocks) 在块末和新语句前插入空行。 jscs: [`requirePaddingNewLinesAfterBlocks`](http://jscs.info/rule/requirePaddingNewLinesAfterBlocks)

    ```javascript
    // bad
    if (foo) {
      return bar;
    }
    return baz;

    // good
    if (foo) {
      return bar;
    }

    return baz;

    // bad
    var obj = {
      foo() {
      },
      bar() {
      },
    };
    return obj;

    // good
    var obj = {
      foo() {
      },

      bar() {
      },
    };

    return obj;

    // bad
    var arr = [
      function foo() {
      },
      function bar() {
      },
    ];
    return arr;

    // good
    var arr = [
      function foo() {
      },

      function bar() {
      },
    ];

    return arr;
    ```

  <a name="whitespace--padded-blocks"></a><a name="10.7"></a>
  - [10.7](#whitespace--padded-blocks) 在块内第一行和最后一行不需要空白行。 eslint: [`padded-blocks`](http://eslint.org/docs/rules/padded-blocks.html) jscs:  [`disallowPaddingNewlinesInBlocks`](http://jscs.info/rule/disallowPaddingNewlinesInBlocks)

    ```javascript
    // bad
    function bar() {

      console.log(foo);

    }

    // also bad
    if (baz) {

      console.log(qux);
    } else {
      console.log(foo);

    }

    // good
    function bar() {
      console.log(foo);
    }

    // good
    if (baz) {
      console.log(qux);
    } else {
      console.log(foo);
    }
    ```

  <a name="whitespace--in-parens"></a><a name="10.8"></a>
  - [10.8](#whitespace--in-parens) 圆括号内不需要空格。 eslint: [`space-in-parens`](http://eslint.org/docs/rules/space-in-parens.html) jscs: [`disallowSpacesInsideParentheses`](http://jscs.info/rule/disallowSpacesInsideParentheses)

    ```javascript
    // bad
    function bar( foo ) {
      return foo;
    }

    // good
    function bar(foo) {
      return foo;
    }

    // bad
    if ( foo ) {
      console.log(foo);
    }

    // good
    if (foo) {
      console.log(foo);
    }
    ```

  <a name="whitespace--in-brackets"></a><a name="10.9"></a>
  - [10.9](#whitespace--in-brackets) 不要再中括号内加空格。 eslint: [`array-bracket-spacing`](http://eslint.org/docs/rules/array-bracket-spacing.html) jscs: [`disallowSpacesInsideArrayBrackets`](http://jscs.info/rule/disallowSpacesInsideArrayBrackets)

    ```javascript
    // bad
    var foo = [ 1, 2, 3 ];
    console.log(foo[ 0 ]);

    // good
    var foo = [1, 2, 3];
    console.log(foo[0]);
    ```

  <a name="whitespace--in-braces"></a><a name="10.10"></a>
  - [10.10](#whitespace--in-braces) 在大括号内添加空格。 eslint: [`object-curly-spacing`](http://eslint.org/docs/rules/object-curly-spacing.html) jscs: [`requireSpacesInsideObjectBrackets`](http://jscs.info/rule/requireSpacesInsideObjectBrackets)

    ```javascript
    // bad
    var foo = {clark: 'kent'};

    // good
    var foo = { clark: 'kent' };
    ```

  <a name="whitespace--max-len"></a><a name="10.11"></a>
  - [10.11](#whitespace--max-len) 避免过长代码。 eslint: [`max-len`](http://eslint.org/docs/rules/max-len.html) jscs: [`maximumLineLength`](http://jscs.info/rule/maximumLineLength)

    > 保持代码可读性

    ```javascript
    // bad
    var foo = jsonData && jsonData.foo && jsonData.foo.bar && jsonData.foo.bar.baz && jsonData.foo.bar.baz.quux && jsonData.foo.bar.baz.quux.xyzzy;

    // bad
    $.ajax({ method: 'POST', url: 'https://airbnb.com/', data: { name: 'John' } }).done(function() {console.log('Congratulations!')}).fail(function() {console.log('You have failed this city.')});

    // good
    const foo = jsonData
      && jsonData.foo
      && jsonData.foo.bar
      && jsonData.foo.bar.baz
      && jsonData.foo.bar.baz.quux
      && jsonData.foo.bar.baz.quux.xyzzy;

    // good
    $.ajax({
      method: 'POST',
      url: 'https://airbnb.com/',
      data: { name: 'John' },
    })
      .done(() => console.log('Congratulations!'))
      .fail(() => console.log('You have failed this city.'));
    ```

**[⬆ back to top](#table-of-contents)**

## 逗号

<a name="commas"></a>
<a name="commas--leading-trailing"></a><a name="12.1"></a>
  - [12.1](#commas--leading-trailing) 行首不需要逗号。 eslint: [`comma-style`](http://eslint.org/docs/rules/comma-style.html) jscs: [`requireCommaBeforeLineBreak`](http://jscs.info/rule/requireCommaBeforeLineBreak)

    ```javascript
    // bad
    var story = [
        once
      , upon
      , aTime
    ];

    // good
    var story = [
      once,
      upon,
      aTime,
    ];

    // bad
    var hero = {
        firstName: 'Ada'
      , lastName: 'Lovelace'
      , birthYear: 1815
      , superPower: 'computers'
    };

    // good
    var hero = {
      firstName: 'Ada',
      lastName: 'Lovelace',
      birthYear: 1815,
      superPower: 'computers',
    };
    ```

**[⬆ back to top](#table-of-contents)**

## 分号

  <a name="semicolons"></a>
  <a name="semicolons--required"></a><a name="13.1"></a>
  - [13.1](#semicolons--required) 使用分号。 eslint: [`semi`](http://eslint.org/docs/rules/semi.html) jscs: [`requireSemicolons`](http://jscs.info/rule/requireSemicolons)

    ```javascript
    // bad
    (function () {
      const name = 'Skywalker'
      return name
    })()

    // good
    (function () {
      const name = 'Skywalker';
      return name;
    }());
    ```

**[⬆ back to top](#table-of-contents)**

## Naming Conventions

  <a name="naming--camelCase"></a><a name="15.2"></a>
  - [15.2](#naming--camelCase) 如果是函数，使用驼峰命名方式。 eslint: [`camelcase`](http://eslint.org/docs/rules/camelcase.html) jscs: [`requireCamelCaseOrUpperCaseIdentifiers`](http://jscs.info/rule/requireCamelCaseOrUpperCaseIdentifiers)

    ```javascript
    // bad
    var OBJEcttsssss = {};
    var this_is_my_object = {};
    function c() {}

    // good
    var thisIsMyObject = {};
    function thisIsMyFunction() {}
    ```

  <a name="naming--PascalCase"></a><a name="15.3"></a>
  - [15.3](#naming--PascalCase) 如果是构造函数，使用帕斯卡命名方式。 eslint: [`new-cap`](http://eslint.org/docs/rules/new-cap.html) jscs: [`requireCapitalizedConstructors`](http://jscs.info/rule/requireCapitalizedConstructors)

    ```javascript
    // bad
    function user(options) {
      this.name = options.name;
    }

    var bad = new user({
      name: 'nope',
    });

    // good
    function User(options) {
      this.name = options.name;
    }

    var good = new User({
      name: 'yup',
    });
    ```

**[⬆ back to top](#table-of-contents)**
