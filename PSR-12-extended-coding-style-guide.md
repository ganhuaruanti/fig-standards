# 程式碼風格規範延伸版

關鍵詞「必須」("MUST")、「絕對不可／絕對不能」("MUST NOT")、「需要」("REQUIRED")、

「將會」("SHALL")、「不會」("SHALL NOT")、「應該」("SHOULD")、「不該」("SHOULD NOT")、

「推薦」("RECOMMENDED")、「可以」("MAY")和「可選」("OPTIONAL")的詳細描述，可參見 [RFC 2119][] 。

[RFC 2119]: http://tools.ietf.org/html/rfc2119

## 概覽

這份規範是針對 [PSR-2][]，程式碼風格規範，的延伸，拓展和取代版本。並且需要附屬於 [PSR-1][]，基礎程式碼規範。

跟 [PSR-2][] 一樣，此規範的用意，在藉由列舉和分享一系列的 PHP 格式規則，減少不同程式撰寫者之間的認知摩擦。這份 PSR 文件希望能在不同專案間，建立排版工具可以實作，專案可以宣布遵守，開發者可以簡單理解的設定方式。

當許多程式撰寫者在不同的專案合作時，針對這些專案制定一個共同的準則是很有幫助的。所以，這份規範的價值不在規則本身，而在散播這份規則上。

[PSR-2][] 在 2012 被接受為正式標準，從那時至今，PHP 語言新增了許多會影響規範的修改。雖然 [PSR-2] 對制定當時的 PHP 有相當全面的規範，但是對這些新功能，實作方式則沒有規範。

因此，這份 PSR 希望可以基於 PSR-2，釐清針對新功能的規範，並針對部分過去的規範進行勘誤。

### 針對之前的語言版本

針對本文件，如果目前開發專案的 PHP 版本還未支援特定指示所講到的功能，**可以**忽略該指示。

### 範例

下面的範例程式簡單地展示了大部分規範：

~~~php
<?php

declare(strict_types=1);

namespace Vendor\Package;

use Vendor\Package\{ClassA as A, ClassB, ClassC as C};
use Vendor\Package\SomeNamespace\ClassD as D;

use function Vendor\Package\{functionA, functionB, functionC};

use const Vendor\Package\{ConstantA, ConstantB, ConstantC};

class Foo extends Bar implements FooInterface
{
    public function sampleFunction(int $a, int $b = null): array
    {
        if ($a === $b) {
            bar();
        } elseif ($a > $b) {
            $foo->bar($arg1);
        } else {
            BazClass::bar($arg2, $arg3);
        }
    }

    final public static function bar()
    {
        // method body
    }
}
~~~

## 2. 通則

### 2.1 基本程式準則

程式碼**必須**遵循 [PSR-1][] 中的規範 。

PSR-1 裡面所說的「駝峰式」（StudlyCaps），**必須**解讀成 Pascal 命名法，也就是命名中每個單字的開頭為大寫，並且整個命名的開頭也是大寫的方式。

### 2.2 檔案

所有 PHP 文件**必須**以 `Unix LF (linefeed)` 作為行的結束符號。

所有 PHP 文件**必須**以一個非空白行，以單一 `LF` 結束的一行作為結尾。

純 PHP 程式檔案**必須**省略最後的 `?>` 標籤。

### 2.3 行

行長度**絕對不可**有硬限制。

行長度的**軟限制**必須是 120 個字元。

一行程式碼的長度**不該**超過 80 個字元。

超過 80 個字元**應該**分成多行，每行不超過 80 個字元。

每行結尾**不該**留有空白。

除了特別禁止的部分之外，**可以**為了增加可讀性和區分相關區塊，加入空白行。

每行**絕對不可**超過一個陳述句。

### 2.4 排版

程式碼**必須**使用四個空格做排版，**絕對不可**使用 tabs 做排版。

### 2.5 關鍵字以及型態

所有 PHP 的關鍵字和型態[[1]][keywords][[2]][types] **必須**使用小寫。

所有未來加入 PHP 新版本的關鍵字和形態**必須**使用小寫。

**必須**使用短版本的型態關鍵字，比方說 `bool` 而不是 `boolean`，`int` 而不是 `integer`⋯⋯等等。

## 3. 宣告，命名空間，以及引入宣告

The header of a PHP file may consist of a number of different blocks. If present,
each of the blocks below MUST be separated by a single blank line, and MUST NOT contain
a blank line. Each block MUST be in the order listed below, although blocks that are
not relevant may be omitted.

* 開頭的 `<?php` 標籤
* 檔案的 docblock
* 一或多個宣告陳述
* 檔案的命名空間宣告
* 一或多個類別的 `use` 引入陳述
* 一或多個函式的 `use` 引入陳述
* 一或多個常數的 `use` 引入陳述
* 其餘的程式碼

如果一個檔案同時包含 HTML 和 PHP，還是可以使用上面所說的任何區塊。如果有使用的話，上面的區塊**必須**

When a file contains a mix of HTML and PHP, any of the above sections may still
be used. If so, they MUST be present at the top of the file, even if the
remainder of the code consists of a closing PHP tag and then a mixture of HTML and
PHP.

When the opening `<?php` tag is on the first line of the file, it MUST be on its
own line with no other statements unless it is a file containing markup outside of PHP
opening and closing tags.

因為必須始終合法，引入陳述**絕對不能**以反斜線開頭。

下面的範例包含上述全部的區塊：

~~~php
<?php

/**
 * 這個檔案包含程式碼風格的範例
 */

declare(strict_types=1);

namespace Vendor\Package;

use Vendor\Package\{ClassA as A, ClassB, ClassC as C};
use Vendor\Package\SomeNamespace\ClassD as D;
use Vendor\Package\AnotherNamespace\ClassE as E;

use function Vendor\Package\{functionA, functionB, functionC};
use function Another\Vendor\functionD;

use const Vendor\Package\{CONSTANT_A, CONSTANT_B, CONSTANT_C};
use const Another\Vendor\CONSTANT_D;

/**
 * FooBar 是一個範例類別
 */
class FooBar
{
    // ... 其餘 PHP 程式碼 ...
}
~~~

**絕對不能**使用超過兩層的複合命名空間，換句話說，下面的案例是允許範圍內層數最多的命名空間

~~~php
<?php

use Vendor\Package\SomeNamespace\{
    SubnamespaceOne\ClassA,
    SubnamespaceOne\ClassB,
    SubnamespaceTwo\ClassY,
    ClassZ,
};
~~~

以下範例則不被允許：

~~~php
<?php

use Vendor\Package\SomeNamespace\{
    SubnamespaceOne\AnotherNamespace\ClassA,
    SubnamespaceOne\ClassB,
    ClassZ,
};
~~~

When wishing to declare strict types in files containing markup outside PHP
opening and closing tags, the declaration MUST be on the first line of the file
and include an opening PHP tag, the strict types declaration and closing tag.

For example:
~~~php
<?php declare(strict_types=1) ?>
<html>
<body>
    <?php
        // ... additional PHP code ...
    ?>
</body>
</html>
~~~

Declare statements MUST contain no spaces and MUST be exactly `declare(strict_types=1)`
(with an optional semi-colon terminator).

Block declare statements are allowed and MUST be formatted as below. Note position of
braces and spacing:
~~~php
declare(ticks=1) {
    // some code
}
~~~

## 4. 類別、屬性和方法

這裡所說的「類別」包含所有的類別，介面，以及 trait。

Any closing brace MUST NOT be followed by any comment or statement on the
same line.

When instantiating a new class, parentheses MUST always be present even when
there are no arguments passed to the constructor.

~~~php
new Foo();
~~~

### 4.1 繼承和實作

The `extends` and `implements` keywords MUST be declared on the same line as
the class name.

The opening brace for the class MUST go on its own line; the closing brace
for the class MUST go on the next line after the body.

Opening braces MUST be on their own line and MUST NOT be preceded or followed
by a blank line.

Closing braces MUST be on their own line and MUST NOT be preceded by a blank
line.

~~~php
<?php

namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class ClassName extends ParentClass implements \ArrayAccess, \Countable
{
    // constants, properties, methods
}
~~~

Lists of `implements` and, in the case of interfaces, `extends` MAY be split
across multiple lines, where each subsequent line is indented once. When doing
so, the first item in the list MUST be on the next line, and there MUST be only
one interface per line.

~~~php
<?php

namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class ClassName extends ParentClass implements
    \ArrayAccess,
    \Countable,
    \Serializable
{
    // constants, properties, methods
}
~~~

### 4.2 使用 trait

The `use` keyword used inside the classes to implement traits MUST be
declared on the next line after the opening brace.

~~~php
<?php

namespace Vendor\Package;

use Vendor\Package\FirstTrait;

class ClassName
{
    use FirstTrait;
}
~~~

Each individual trait that is imported into a class MUST be included
one-per-line and each inclusion MUST have its own `use` import statement.

~~~php
<?php

namespace Vendor\Package;

use Vendor\Package\FirstTrait;
use Vendor\Package\SecondTrait;
use Vendor\Package\ThirdTrait;

class ClassName
{
    use FirstTrait;
    use SecondTrait;
    use ThirdTrait;
}
~~~

When the class has nothing after the `use` import statement, the class
closing brace MUST be on the next line after the `use` import statement.

~~~php
<?php

namespace Vendor\Package;

use Vendor\Package\FirstTrait;

class ClassName
{
    use FirstTrait;
}
~~~

Otherwise, it MUST have a blank line after the `use` import statement.

~~~php
<?php

namespace Vendor\Package;

use Vendor\Package\FirstTrait;

class ClassName
{
    use FirstTrait;

    private $property;
}
~~~

When using the `insteadof` and `as` operators they must be used as follows taking
note of indentation, spacing, and new lines.

~~~php
<?php

class Talker
{
    use A, B, C {
        B::smallTalk insteadof A;
        A::bigTalk insteadof C;
        C::mediumTalk as FooBar;
    }
}
~~~

### 4.3 屬性和常數

Visibility MUST be declared on all properties.

Visibility MUST be declared on all constants if your project PHP minimum
version supports constant visibilities (PHP 7.1 or later).

The `var` keyword MUST NOT be used to declare a property.

There MUST NOT be more than one property declared per statement.

Property names MUST NOT be prefixed with a single underscore to indicate
protected or private visibility. That is, an underscore prefix explicitly has
no meaning.

There MUST be a space between type declaration and property name.

A property declaration looks like the following:

~~~php
<?php

namespace Vendor\Package;

class ClassName
{
    public $foo = null;
    public static int $bar = 0;
}
~~~

### 4.4 方法和函式

Visibility MUST be declared on all methods.

Method names MUST NOT be prefixed with a single underscore to indicate
protected or private visibility. That is, an underscore prefix explicitly has
no meaning.

Method and function names MUST NOT be declared with space after the method name. The
opening brace MUST go on its own line, and the closing brace MUST go on the
next line following the body. There MUST NOT be a space after the opening
parenthesis, and there MUST NOT be a space before the closing parenthesis.

A method declaration looks like the following. Note the placement of
parentheses, commas, spaces, and braces:

~~~php
<?php

namespace Vendor\Package;

class ClassName
{
    public function fooBarBaz($arg1, &$arg2, $arg3 = [])
    {
        // method body
    }
}
~~~

A function declaration looks like the following. Note the placement of
parentheses, commas, spaces, and braces:

~~~php
<?php

function fooBarBaz($arg1, &$arg2, $arg3 = [])
{
    // function body
}
~~~

### 4.5 Method and Function Arguments

In the argument list, there MUST NOT be a space before each comma, and there
MUST be one space after each comma.

Method and function arguments with default values MUST go at the end of the argument
list.

~~~php
<?php

namespace Vendor\Package;

class ClassName
{
    public function foo(int $arg1, &$arg2, $arg3 = [])
    {
        // method body
    }
}
~~~

Argument lists MAY be split across multiple lines, where each subsequent line
is indented once. When doing so, the first item in the list MUST be on the
next line, and there MUST be only one argument per line.

When the argument list is split across multiple lines, the closing parenthesis
and opening brace MUST be placed together on their own line with one space
between them.

~~~php
<?php

namespace Vendor\Package;

class ClassName
{
    public function aVeryLongMethodName(
        ClassTypeHint $arg1,
        &$arg2,
        array $arg3 = []
    ) {
        // method body
    }
}
~~~

When you have a return type declaration present, there MUST be one space after
the colon followed by the type declaration. The colon and declaration MUST be
on the same line as the argument list closing parenthesis with no spaces between
the two characters.

~~~php
<?php

declare(strict_types=1);

namespace Vendor\Package;

class ReturnTypeVariations
{
    public function functionName(int $arg1, $arg2): string
    {
        return 'foo';
    }

    public function anotherFunction(
        string $foo,
        string $bar,
        int $baz
    ): string {
        return 'foo';
    }
}
~~~

In nullable type declarations, there MUST NOT be a space between the question mark
and the type.

~~~php
<?php

declare(strict_types=1);

namespace Vendor\Package;

class ReturnTypeVariations
{
    public function functionName(?string $arg1, ?int &$arg2): ?string
    {
        return 'foo';
    }
}
~~~

When using the reference operator `&` before an argument, there MUST NOT be
a space after it, like in the previous example.

There MUST NOT be a space between the variadic three dot operator and the argument
name:

```php
public function process(string $algorithm, ...$parts)
{
    // processing
}
```

When combining both the reference operator and the variadic three dot operator,
there MUST NOT be any space between the two of them:

```php
public function process(string $algorithm, &...$parts)
{
    // processing
}
```

### 4.6 `abstract`, `final`, and `static`

When present, the `abstract` and `final` declarations MUST precede the
visibility declaration.

When present, the `static` declaration MUST come after the visibility
declaration.

~~~php
<?php

namespace Vendor\Package;

abstract class ClassName
{
    protected static $foo;

    abstract protected function zim();

    final public static function bar()
    {
        // method body
    }
}
~~~

### 4.7 方法和函式呼叫

When making a method or function call, there MUST NOT be a space between the
method or function name and the opening parenthesis, there MUST NOT be a space
after the opening parenthesis, and there MUST NOT be a space before the
closing parenthesis. In the argument list, there MUST NOT be a space before
each comma, and there MUST be one space after each comma.

~~~php
<?php

bar();
$foo->bar($arg1);
Foo::bar($arg2, $arg3);
~~~

Argument lists MAY be split across multiple lines, where each subsequent line
is indented once. When doing so, the first item in the list MUST be on the
next line, and there MUST be only one argument per line. A single argument being
split across multiple lines (as might be the case with an anonymous function or
array) does not constitute splitting the argument list itself.

~~~php
<?php

$foo->bar(
    $longArgument,
    $longerArgument,
    $muchLongerArgument
);
~~~

~~~php
<?php

somefunction($foo, $bar, [
  // ...
], $baz);

$app->get('/hello/{name}', function ($name) use ($app) {
    return 'Hello ' . $app->escape($name);
});
~~~

## 5. 控制結構

The general style rules for control structures are as follows:

- There MUST be one space after the control structure keyword
- There MUST NOT be a space after the opening parenthesis
- There MUST NOT be a space before the closing parenthesis
- There MUST be one space between the closing parenthesis and the opening
  brace
- The structure body MUST be indented once
- The body MUST be on the next line after the opening brace
- The closing brace MUST be on the next line after the body

The body of each structure MUST be enclosed by braces. This standardizes how
the structures look and reduces the likelihood of introducing errors as new
lines get added to the body.

### 5.1 `if`，`elseif`，`else`

An `if` structure looks like the following. Note the placement of parentheses,
spaces, and braces; and that `else` and `elseif` are on the same line as the
closing brace from the earlier body.

~~~php
<?php

if ($expr1) {
    // if body
} elseif ($expr2) {
    // elseif body
} else {
    // else body;
}
~~~

The keyword `elseif` SHOULD be used instead of `else if` so that all control
keywords look like single words.

Expressions in parentheses MAY be split across multiple lines, where each
subsequent line is indented at least once. When doing so, the first condition
MUST be on the next line. The closing parenthesis and opening brace MUST be
placed together on their own line with one space between them. Boolean
operators between conditions MUST always be at the beginning or at the end of
the line, not a mix of both.

~~~php
<?php

if (
    $expr1
    && $expr2
) {
    // if body
} elseif (
    $expr3
    && $expr4
) {
    // elseif body
}
~~~

### 5.2 `switch`，`case`

A `switch` structure looks like the following. Note the placement of
parentheses, spaces, and braces. The `case` statement MUST be indented once
from `switch`, and the `break` keyword (or other terminating keywords) MUST be
indented at the same level as the `case` body. There MUST be a comment such as
`// no break` when fall-through is intentional in a non-empty `case` body.

~~~php
<?php

switch ($expr) {
    case 0:
        echo 'First case, with a break';
        break;
    case 1:
        echo 'Second case, which falls through';
        // no break
    case 2:
    case 3:
    case 4:
        echo 'Third case, return instead of break';
        return;
    default:
        echo 'Default case';
        break;
}
~~~

Expressions in parentheses MAY be split across multiple lines, where each
subsequent line is indented at least once. When doing so, the first condition
MUST be on the next line. The closing parenthesis and opening brace MUST be
placed together on their own line with one space between them. Boolean
operators between conditions MUST always be at the beginning or at the end of
the line, not a mix of both.

~~~php
<?php

switch (
    $expr1
    && $expr2
) {
    // structure body
}
~~~

### 5.3 `while`, `do while`

A `while` statement looks like the following. Note the placement of
parentheses, spaces, and braces.

~~~php
<?php

while ($expr) {
    // structure body
}
~~~

Expressions in parentheses MAY be split across multiple lines, where each
subsequent line is indented at least once. When doing so, the first condition
MUST be on the next line. The closing parenthesis and opening brace MUST be
placed together on their own line with one space between them. Boolean
operators between conditions MUST always be at the beginning or at the end of
the line, not a mix of both.

~~~php
<?php

while (
    $expr1
    && $expr2
) {
    // structure body
}
~~~

Similarly, a `do while` statement looks like the following. Note the placement
of parentheses, spaces, and braces.

~~~php
<?php

do {
    // structure body;
} while ($expr);
~~~

Expressions in parentheses MAY be split across multiple lines, where each
subsequent line is indented at least once. When doing so, the first condition
MUST be on the next line. Boolean operators between conditions MUST
always be at the beginning or at the end of the line, not a mix of both.

~~~php
<?php

do {
    // structure body;
} while (
    $expr1
    && $expr2
);
~~~

### 5.4 `for`

A `for` statement looks like the following. Note the placement of parentheses,
spaces, and braces.

~~~php
<?php

for ($i = 0; $i < 10; $i++) {
    // for body
}
~~~

Expressions in parentheses MAY be split across multiple lines, where each
subsequent line is indented at least once. When doing so, the first expression
MUST be on the next line. The closing parenthesis and opening brace MUST be
placed together on their own line with one space between them.

~~~php
<?php

for (
    $i = 0;
    $i < 10;
    $i++
) {
    // for body
}
~~~

### 5.5 `foreach`

A `foreach` statement looks like the following. Note the placement of
parentheses, spaces, and braces.

~~~php
<?php

foreach ($iterable as $key => $value) {
    // foreach body
}
~~~

### 5.6 `try`，`catch`，`finally`

A `try-catch-finally` block looks like the following. Note the placement of
parentheses, spaces, and braces.

~~~php
<?php

try {
    // try body
} catch (FirstThrowableType $e) {
    // catch body
} catch (OtherThrowableType | AnotherThrowableType $e) {
    // catch body
} finally {
    // finally body
}
~~~

## 6. 運算子

Style rules for operators are grouped by arity (the number of operands they take).

When space is permitted around an operator, multiple spaces MAY be
used for readability purposes.

All operators not described here are left undefined.

### 6.1. 一元運算

The increment/decrement operators MUST NOT have any space between
the operator and operand.
~~~php
$i++;
++$j;
~~~

Type casting operators MUST NOT have any space within the parentheses:
~~~php
$intValue = (int) $input;
~~~

### 6.2. 二元運算

All binary [arithmetic][], [comparison][], [assignment][], [bitwise][],
[logical][], [string][], and [type][] operators MUST be preceded and
followed by at least one space:
~~~php
if ($a === $b) {
    $foo = $bar ?? $a ?? $b;
} elseif ($a > $b) {
    $foo = $a + $b * $c;
}
~~~

### 6.3. 三元運算

The conditional operator, also known simply as the ternary operator, MUST be
preceded and followed by at least one space around both the `?`
and `:` characters:
~~~php
$variable = $foo ? 'foo' : 'bar';
~~~

When the middle operand of the conditional operator is omitted, the operator
MUST follow the same style rules as other binary [comparison][] operators:
~~~php
$variable = $foo ?: 'bar';
~~~

## 7. 閉包

Closures MUST be declared with a space after the `function` keyword, and a
space before and after the `use` keyword.

The opening brace MUST go on the same line, and the closing brace MUST go on
the next line following the body.

There MUST NOT be a space after the opening parenthesis of the argument list
or variable list, and there MUST NOT be a space before the closing parenthesis
of the argument list or variable list.

In the argument list and variable list, there MUST NOT be a space before each
comma, and there MUST be one space after each comma.

Closure arguments with default values MUST go at the end of the argument
list.

If a return type is present, it MUST follow the same rules as with normal
functions and methods; if the `use` keyword is present, the colon MUST follow
the `use` list closing parentheses with no spaces between the two characters.

A closure declaration looks like the following. Note the placement of
parentheses, commas, spaces, and braces:

~~~php
<?php

$closureWithArgs = function ($arg1, $arg2) {
    // body
};

$closureWithArgsAndVars = function ($arg1, $arg2) use ($var1, $var2) {
    // body
};

$closureWithArgsVarsAndReturn = function ($arg1, $arg2) use ($var1, $var2): bool {
    // body
};
~~~

Argument lists and variable lists MAY be split across multiple lines, where
each subsequent line is indented once. When doing so, the first item in the
list MUST be on the next line, and there MUST be only one argument or variable
per line.

When the ending list (whether of arguments or variables) is split across
multiple lines, the closing parenthesis and opening brace MUST be placed
together on their own line with one space between them.

The following are examples of closures with and without argument lists and
variable lists split across multiple lines.

~~~php
<?php

$longArgs_noVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) {
   // body
};

$noArgs_longVars = function () use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // body
};

$longArgs_longVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // body
};

$longArgs_shortVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) use ($var1) {
   // body
};

$shortArgs_longVars = function ($arg) use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // body
};
~~~

Note that the formatting rules also apply when the closure is used directly
in a function or method call as an argument.

~~~php
<?php

$foo->bar(
    $arg1,
    function ($arg2) use ($var1) {
        // body
    },
    $arg3
);
~~~

## 8. 匿名類別

Anonymous Classes MUST follow the same guidelines and principles as closures
in the above section.

~~~php
<?php

$instance = new class {};
~~~

The opening brace MAY be on the same line as the `class` keyword so long as
the list of `implements` interfaces does not wrap. If the list of interfaces
wraps, the brace MUST be placed on the line immediately following the last
interface.

~~~php
<?php

// Brace on the same line
$instance = new class extends \Foo implements \HandleableInterface {
    // Class content
};

// Brace on the next line
$instance = new class extends \Foo implements
    \ArrayAccess,
    \Countable,
    \Serializable
{
    // Class content
};
~~~

[PSR-1]: http://www.php-fig.org/psr/psr-1/
[PSR-2]: http://www.php-fig.org/psr/psr-2/
[keywords]: http://php.net/manual/en/reserved.keywords.php
[types]: http://php.net/manual/en/reserved.other-reserved-words.php
[arithmetic]: http://php.net/manual/en/language.operators.arithmetic.php
[assignment]: http://php.net/manual/en/language.operators.assignment.php
[comparison]: http://php.net/manual/en/language.operators.comparison.php
[bitwise]: http://php.net/manual/en/language.operators.bitwise.php
[logical]: http://php.net/manual/en/language.operators.logical.php
[string]: http://php.net/manual/en/language.operators.string.php
[type]: http://php.net/manual/en/language.operators.type.php
