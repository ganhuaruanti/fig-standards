# 程式碼風格規範延伸版

關鍵詞「必須」("MUST")、「絕對不可／絕對不能」("MUST NOT")、「需要」("REQUIRED")、

「將會」("SHALL")、「不會」("SHALL NOT")、「應該」("SHOULD")、「不該」("SHOULD NOT")、

「推薦」("RECOMMENDED")、「可以」("MAY")和「可選」("OPTIONAL")的詳細描述，可參見 [RFC 2119][] 。

[RFC 2119]: http://tools.ietf.org/html/rfc2119

## 概覽

這份規範是針對 [PSR-2][]——程式碼風格規範——的延伸、拓展和取代版本。並且需要附屬於 [PSR-1][]，基礎程式碼規範。

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
        // 函式主體
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

PHP 檔案開頭可能包含許多不同的區塊。每個出現的區塊**必須**以一空白行做區隔，但每個區塊內**絕對不能**包含空白行。

如果不需要的話，可以省略特定區塊，但是出現的順序**必須**如下面排列：

* 開頭的 `<?php` 標籤
* 檔案的 docblock 註解
* 一或多個宣告陳述
* 檔案的命名空間宣告
* 一或多個類別的 `use` 引入陳述
* 一或多個函式的 `use` 引入陳述
* 一或多個常數的 `use` 引入陳述
* 其餘的程式碼

如果一個檔案同時包含 HTML 和 PHP，還是可以使用上面所說的任何區塊。如果有使用的話，上面的區塊**必須**放在檔案開頭。

當一個檔案同時包含 HTML 和 PHP 程式碼時，還是可以包含上面所說的區塊。不過，包含時即使頁面混雜不同的 PHP 和 HTML 段落，上面所說的區塊**必須**全部放在檔案開頭。

當以 `<?php` 標籤作為檔案開頭時，**必須**自己獨立一行，沒有其他宣告。除非這個檔案除了 PHP 開頭到結尾標籤以外，還包含其他內容。

因為語句必須從頭到尾合法，引入陳述**絕對不能**以反斜線開頭。

以下的範例包含上面所說全部的區塊：

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

如果希望在一個除了 PHP 開頭到結尾標籤以外，還包含其他內容的檔案裡面，宣告強型別（strict types）的話，宣告的位置**必須**在檔案的第一行，該行同時包含 PHP 開頭標籤，強型別的宣告，以及結尾標籤。

像是：
~~~php
<?php declare(strict_types=1) ?>
<html>
<body>
    <?php
        // ... 其餘 PHP 程式碼 ...
    ?>
</body>
</html>
~~~

強型別的宣告**必須**不包含任何空白，並且**必須**寫成 `declare(strict_types=1)`（可以在最尾端加上分號）。

區塊宣告是允許的，並且**必須**是以下的格式。注意大括號和空白的位置：

~~~php
declare(ticks=1) {
    // 一些程式碼
}
~~~

## 4. 類別、屬性和方法

這裡所說的「類別」包含所有的類別，介面，以及 trait。

所有右大括號**絕對不能**在同一行緊接著任何註解或者宣告。

建立新物件時，即使建構子不需要任何參數，還是**必須**出現括弧。

~~~php
new Foo();
~~~

### 4.1 繼承和實作

`extends` 和 `implements` 關鍵字**必須**和類別名稱宣告於同一行。

類別的左大括號**必須**獨立成為一行；類別的右大括號**必須**在類別主體的下一行。

左大括號**必須**獨立成為一行，並且**絕對不能**在前面或後面加上空白行。

右大括號**必須**獨立成為一行，並且**絕對不能**在前面加上空白行。

~~~php
<?php

namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class ClassName extends ParentClass implements \ArrayAccess, \Countable
{
    // 常數，參數，函式
}
~~~

介面的 `implements` 和 `extends` **可以**拆分成多行，每一行都縮排一次。

這樣做的時候，第一個實作的介面**必須**在類別宣告的下一行。每一行**必須**只包含一個介面。

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
    // 常數，參數，函式
}
~~~

### 4.2 使用 trait

類別裡宣告 trait 用的 `use` 關鍵字，**必須**宣告在類別左大括弧的下一行。

~~~php
<?php

namespace Vendor\Package;

use Vendor\Package\FirstTrait;

class ClassName
{
    use FirstTrait;
}
~~~

每個 trait **必須**個別在每行宣告，並且每行**必須**有獨立的 `use` 宣告。

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

當類別除了 `use` 宣告之外不包含任何東西時，類別的右大括弧**必須**緊接在 `use` 宣告的下一行。

~~~php
<?php

namespace Vendor\Package;

use Vendor\Package\FirstTrait;

class ClassName
{
    use FirstTrait;
}
~~~

不然，在 `use` 宣告下面**必須**包含一行空白行。

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

使用 `insteadof` 和 `as` 關鍵字時，必須以下面的縮排方式宣告。注意排版，空白和換行的位置：

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

所有的屬性**必須**宣告可見度。

如果專案的 PHP 版本支援常數宣告可見度（PHP 7.1 之後的版本），那麼所有常數**必須**要宣告可見度。

**絕對不能**使用 `var` 關鍵字來宣告屬性。

每個宣告**絕對不能**包含超過一個屬性。

**絕對不能**使用單一底線前綴來代表屬性是 protected 或者 private。前綴底線不應該代表任何意義。

型態宣告和屬性名稱之間**必須**要加上空白。

屬性宣告的範例如下：

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

可見度**必須**在所有函式前面宣告。

**絕對不能**使用單一底線前綴來代表函式是 protected 或者 private。前綴底線不應該代表任何意義。

函式名稱的後面**絕對不能**加上任何空白。

右大括號**必須**獨立一行，左大括號**必須**在主體的下一行。

右大括號後面**絕對不能**有任何空白，左大括號前面**絕對不能**有任何空白。

方法宣告方式如下，要特別注意括弧和逗號的位置：

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

函式宣告方式如下，要特別注意括弧和逗號的位置：

~~~php
<?php

function fooBarBaz($arg1, &$arg2, $arg3 = [])
{
    // function body
}
~~~

### 4.5 函式參數

在參數列表內，每個逗號前面**絕對不能**包含空白，逗號後面**必須**包含一個空白。

函式和方法的參數列表內，如果參數有預設值，這些參數**必須**放在參數列表的最尾端。

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

參數列表**可以**分成多行，每行縮排一次。

這樣處理的話，第一個參數**必須**在函式名稱的下一行，並且每行**必須**只有一個參數

當參數區分成多行時，如果沒有宣告回傳型態，右小括弧和左大括弧**必須**宣告在同一行，並且中間以一個空白間隔。

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
        // 方法主體
    }
}
~~~

如果有宣告回傳型態的話，分號和型態宣告之間**必須**加上一個空白，分號和宣告**必須**和參數列表的右小括號同一行，並且分號和右小括號之間沒有空白。

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

將型態宣告為可為空值（nullable）時，問號和型態宣告之間**絕對不能**有空白。

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

像前面的範例一樣，如果參數前面有使用參考的操作子 `&`，那麼 `&` 後面絕對不能加上空白。

可變參數的三個點和參數名稱之間**絕對不能**有空白：

```php
public function process(string $algorithm, ...$parts)
{
    // processing
}
```
合併參考的操作子 `&` 和可變參數的三個點之間，**絕對不能**包含任何空白

```php
public function process(string $algorithm, &...$parts)
{
    // processing
}
```

### 4.6 `abstract`，`final` 和 `static`

如果有宣告 `abstract` 和 `final`，**必須**緊接在可見度宣告之前。

如果有宣告 `static`，**必須**緊接在可見度宣告之後。

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

呼叫方法或函式時，名稱和左小括號之間**絕對不能**有任何空白。左小括號之後**絕對不能**有任何空白。右小括號之前**絕對不能**有任何空白。

參數列表內，逗號前面**絕對不能**有任何空白，逗號後面**必須**有一個空白。

~~~php
<?php

bar();
$foo->bar($arg1);
Foo::bar($arg2, $arg3);
~~~

參數列表**可以**分成多行，每行縮排一次。

這樣處理的話，第一個參數**必須**在函式名稱的下一行，並且每行**必須**只有一個參數

單一參數切分成多行的情況（比方說使用匿名函式或者陣列為參數時）並不屬於將參數列表拆分成多行的情況。

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

控制結構的基本規則如下：

- 控制結構關鍵字的後方**必須**要有一個空格
- 左小括弧的後方**絕對不能**有空白
- 右小括弧的前方**絕對不能**有空白
- 右小括弧和左大括弧之間**必須**要有一個空白
- 結構主體**必須**縮排一次
- 結構主體**必須**在左大括弧的下面一行
- 右大括弧**必須**在主體的下面一行

每個主體**必須**以大括弧包起來。這樣可以讓控制主體的外觀標準化，並減少加入新程式碼時出錯的機率。

### 5.1 `if`，`elseif`，`else`

`if` 結構**必須**看起來像下面範例。注意括號和空白的位置。特別是 `else` 和 `elseif` 關鍵字位在上一個主體下面的右大括號。

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

**應該**使用 `elseif` 而不是 `else if`，這樣所有的控制關鍵字看起來都會是一個單字。

小括號內的判斷表達**可以**分成多行。分成多行時，判斷表達應該往右縮排一次。

第一個判斷式**必須**在左小括弧下一行。

右小括弧和左大括弧**必須**在同一行，該行只有這兩個符號，並且中間以一個空白隔開。

條件之間的布林運算式**必須**全部在判斷式的前面或全部在判斷式的後面，不可以交錯出現。

~~~php
<?php

if (
    $expr1
    && $expr2
) {
    // if 主體
} elseif (
    $expr3
    && $expr4
) {
    // elseif 主體
}
~~~

### 5.2 `switch`，`case`

`switch` 結構如下，注意括弧和空白的位置。

`case` 的位置**必須**從 `switch` 往右縮排一次。

`break` 以及其他終止判斷關鍵字**必須**與 `case` 主體縮排位置相同。

在有邏輯的 `case` 主體內，如果刻意不用終止判斷關鍵字，讓程式可以繼續往下執行的話，**必須**加上 `// no break` 之類的註解


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

小括號內的判斷表達**可以**分成多行。分成多行時，判斷表達應該往右縮排一次。

第一個判斷式**必須**在左小括弧下一行。

右小括弧和左大括弧**必須**在同一行，該行只有這兩個符號，並且中間以一個空白隔開。

條件之間的布林運算式**必須**全部在判斷式的前面或全部在判斷式的後面，不可以交錯出現。

~~~php
<?php

switch (
    $expr1
    && $expr2
) {
    // structure body
}
~~~

### 5.3 `while`，`do while`

`while` 結構如下，注意括弧和空白的位置。

~~~php
<?php

while ($expr) {
    // structure body
}
~~~

小括號內的判斷表達**可以**分成多行。分成多行時，判斷表達應該往右縮排一次。

第一個判斷式**必須**在左小括弧下一行。

右小括弧和左大括弧**必須**在同一行，該行只有這兩個符號，並且中間以一個空白隔開。

條件之間的布林運算式**必須**全部在判斷式的前面或全部在判斷式的後面，不可以交錯出現。

~~~php
<?php

while (
    $expr1
    && $expr2
) {
    // structure body
}
~~~

`do while` 的規範類似 `while`，結構如下。注意括弧和空白的位置。

~~~php
<?php

do {
    // structure body;
} while ($expr);
~~~

小括號內的判斷表達**可以**分成多行。分成多行時，判斷表達應該往右縮排一次。

第一個判斷式**必須**在左小括弧下一行。

右小括弧和左大括弧**必須**在同一行，該行只有這兩個符號，並且中間以一個空白隔開。

條件之間的布林運算式**必須**全部在判斷式的前面或全部在判斷式的後面，不可以交錯出現。

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

`for` 結構如下，注意括弧和空白的位置。

~~~php
<?php

for ($i = 0; $i < 10; $i++) {
    // for body
}
~~~

小括號內的判斷表達**可以**分成多行。分成多行時，判斷表達應該往右縮排一次。

第一個判斷式**必須**在左小括弧下一行。

右小括弧和左大括弧**必須**在同一行，該行只有這兩個符號，並且中間以一個空白隔開。

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

`foreach` 宣告方式如下，注意括號以及空白的位置：

~~~php
<?php

foreach ($iterable as $key => $value) {
    // foreach 主體
}
~~~

### 5.6 `try`，`catch`，`finally`

`try-catch-finally` 區塊宣告方式如下，注意括號以及空白的位置：

~~~php
<?php

try {
    // try 主體
} catch (FirstThrowableType $e) {
    // catch 主體
} catch (OtherThrowableType | AnotherThrowableType $e) {
    // catch 主體
} finally {
    // finally 主體
}
~~~

## 6. 運算子

以下規則以參數數量（該運算子會碰觸到的運算元數量）分類。

如果允許運算子附近有空格時，**可以**使用多個空格來增加可讀性。

所有這邊沒提到的運算子視為未定義。

### 6.1. 一元運算

遞增／遞減運算子**絕對不能**和運算元之間有任何空格。

~~~php
$i++;
++$j;
~~~

型態轉換運算子**絕對不能**和小括弧之間有任何空格

~~~php
$intValue = (int) $input;
~~~

### 6.2. 二元運算

所有二元的 [運算][] ， [比較][] ， [賦值][] ， [位元處理][] ， [邏輯][] ， [字串][] 以及型態 [type][] 運算子**必須**前後加上一個空白：

~~~php
if ($a === $b) {
    $foo = $bar ?? $a ?? $b;
} elseif ($a > $b) {
    $foo = $a + $b * $c;
}
~~~

### 6.3. 三元運算

三元條件運算子必須在 `?` 和 `:` 前後都各加上一個空白：

~~~php
$variable = $foo ? 'foo' : 'bar';
~~~

如果省略掉中間的運算元，變成 `?:` 的結構，這個運算子**必須**遵守二元 [比較][] 的規則：

~~~php
$variable = $foo ?: 'bar';
~~~

## 7. 閉包

閉包**必須**在 `function` 關鍵字之後加上一個空格，並且在 `use` 關鍵字前後均加上一個空格。

右大括號**必須**在關鍵字同一行，左大括號**必須**要在主體的下一行。

左小括號和第一個參數之間**絕對不能**插入空白。右小括號和最後一個參數之間**絕對不能**插入空白。

參數列表裡面，逗號前面**絕對不能**有空白，逗號後面**必須**有一個空白。

參數列表裡面有預設值的參數**必須**放在列表的最後面。


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

匿名類別**必須**符合上面針對匿名函式的原則以及方針。

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
