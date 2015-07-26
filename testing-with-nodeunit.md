#使用 Nodeunit 测试
  
##问题
  
假如你正在使用 CoffeeScript 并且想要验证功能是否与预期一致，便可以决定使用 Nodeunit 测试框架。
  
##讨论
  
Nodeunit 是一种 JavaScript 对于 单元测试库（ Unit Testing libraries ）中xUnit 族的实现，Java, Python, Ruby, Smalltalk 中均可以使用。
  
当使用 xUnit 族测试框架时，你需要将所需测试的描述预期功能的代码写在一个文件中。
  
例如，我们希望我们的计算器可以进行加法和减法，并且对于正负数均可以正确计算，我们的测试如下。
  
<pre><code>
# test/calculator.test.coffee
Calculator = require '../calculator'
exports.CalculatorTest =
    'test can add two positive numbers': (test) ->
        calculator = new Calculator
        result = calculator.add 2, 3
        test.equal(result, 5)
        test.done()

    'test can handle negative number addition': (test) ->
        calculator = new Calculator
        result = calculator.add -10, 5
        test.equal(result,  -5)
        test.done()

    'test can subtract two positive numbers': (test) ->
        calculator = new Calculator
        result = calculator.subtract 10, 6
        test.equal(result, 4)
        test.done()

    'test can handle negative number subtraction': (test) ->
        calculator = new Calculator
        result = calculator.subtract 4, -6
        test.equal(result, 10)
        test.done()
</code></pre>
  
**安装 Nodeunit**
  
在可以运行你的测试之前，你必须先安装  Nodeunit ：
  
首先创建一个 package.json 文件
  
<pre><code>
{
  "name": "calculator",
  "version": "0.0.1",
  "scripts": {
    "test": "./node_modules/.bin/nodeunit test"
  },
  "dependencies": {
    "coffee-script": "~1.4.0",
    "nodeunit": "~0.7.4"
  }
}
</code></pre>
  
接下来从一个终端运行。
  
<pre><code>
$ npm install
</code></pre>
  
**运行测试**
  
使用代码行可以简便地运行测试文件：


<pre><code>
$ npm test
</code></pre>
  

测试失败，因为我们并没有 calculator.coffee


<pre><code>
suki@Yuzuki:nodeunit_testing (master)$ npm test
npm WARN package.json calculator@0.0.1 No README.md file found!

> calculator@0.0.1 test /Users/suki/tmp/nodeunit_testing
> ./node_modules/.bin/nodeunit test


/Users/suki/tmp/nodeunit_testing/node_modules/nodeunit/lib/nodeunit.js:72
        if (err) throw err;
                       ^
Error: ENOENT, stat '/Users/suki/tmp/nodeunit_testing/test'
npm ERR! Test failed.  See above for more details.
npm ERR! not ok code 0
</code></pre>
  
我们创建一个简单文件
<pre><code>
# calculator.coffee

class Calculator

module.exports = Calculator
</code></pre>
  
并且重新运行测试套件。

<pre><code>
suki@Yuzuki:nodeunit_testing (master)$ npm test
npm WARN package.json calculator@0.0.1 No README.md file found!

> calculator@0.0.1 test /Users/suki/tmp/nodeunit_testing
> ./node_modules/.bin/nodeunit test


calculator.test
✖ CalculatorTest - test can add two positive numbers

TypeError: Object #<Calculator> has no method 'add'
  ...

✖ CalculatorTest - test can handle negative number addition

TypeError: Object #<Calculator> has no method 'add'
  ...

✖ CalculatorTest - test can subtract two positive numbers

TypeError: Object #<Calculator> has no method 'subtract'
  ...

✖ CalculatorTest - test can handle negative number subtraction

TypeError: Object #<Calculator> has no method 'subtract'
  ...


FAILURES: 4/4 assertions failed (31ms)
npm ERR! Test failed.  See above for more details.
npm ERR! not ok code 0
</code></pre>
  
**通过测试**
  
让我们对方法进行实现来观察测试是否可以通过。
  
<pre><code>
# calculator.coffee

class Calculator

  add: (a, b) ->
    a + b

  subtract: (a, b) ->
    a - b

module.exports = Calculator
</code></pre>
  
当我们重新运行测试时可以看到全部通过：
<pre><code>
suki@Yuzuki:nodeunit_testing (master)$ npm test
npm WARN package.json calculator@0.0.1 No README.md file found!

> calculator@0.0.1 test /Users/suki/tmp/nodeunit_testing
> ./node_modules/.bin/nodeunit test


calculator.test
✔ CalculatorTest - test can add two positive numbers
✔ CalculatorTest - test can handle negative number addition
✔ CalculatorTest - test can subtract two positive numbers
✔ CalculatorTest - test can handle negative number subtraction

OK: 4 assertions (27ms)
</code></pre>
  
**重构测试**
  
既然测试全部通过，我们应看一看我们的代码或测试是否可以被重构。
  
在我们的测试文件中，每个测试都创建了自己的 calculator 实例。这会使我们的测试相当的重复，特别是对于大型的测试套件。理想情况下，我们应该考虑将初始化代码移动到每次测试之前运行。
  
通常在其他的 xUnit 库中，Nodeunit 会提供一个 setUp（以及 tearDown ）功能会在测试前调用。
  
<pre><code>
Calculator = require '../calculator'

exports.CalculatorTest =

    setUp: (callback) ->
        @calculator = new Calculator
        callback()

    'test can add two positive numbers': (test) ->
        result = @calculator.add 2, 3
        test.equal(result, 5)
        test.done()

    'test can handle negative number addition': (test) ->
        result = @calculator.add -10, 5
        test.equal(result,  -5)
        test.done()

    'test can subtract two positive numbers': (test) ->
        result = @calculator.subtract 10, 6
        test.equal(result, 4)
        test.done()

    'test can handle negative number subtraction': (test) ->
        result = @calculator.subtract 4, -6
        test.equal(result, 10)
        test.done()
</code></pre>
  
我们可以重新运行测试，仍然可以全部通过。









