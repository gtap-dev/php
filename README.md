# PHP Coding Style Guide

A mostly reasonable approach to PHP

This is a superset of the [PSR-12].

## 1. Overview

### 1.1. Previous language versions

Throughout this document, any instructions may be ignored if they do not exist in versions
of PHP supported by your project.

## 2. Basic Coding Standard

## 2.1 Overview

- Files must use only `<?php` tags.

- Files must use only UTF-8 without BOM for PHP code.

- Files should *either* declare symbols (classes, functions, constants, etc.)
  *or* cause side-effects (e.g. generate output, change .ini settings, etc.)
  but should not do both.

- Class names must be declared in `PascalCase`.

- Class constants must be declared in all upper case with underscore separators.

- Class property names should be declared in `camelCase`.

- Method names must be declared in `camelCase`.

## 2.2 Autoloader

- The term "class" refers to classes, interfaces, traits, and other similar
   structures.

- A fully qualified class name has the following form:

        \<NamespaceName>(\<SubNamespaceNames>)*\<ClassName>

- The fully qualified class name must have a top-level namespace name,
   also known as a "vendor namespace".

- The fully qualified class name may have one or more sub-namespace
   names.

- The fully qualified class name must have a terminating class name.

- Underscores have no special meaning in any portion of the fully
   qualified class name.

- Alphabetic characters in the fully qualified class name may be any
   combination of lower case and upper case.

- All class names must be referenced in a case-sensitive fashion.

- When loading a file that corresponds to a fully qualified class name ...

- A contiguous series of one or more leading namespace and sub-namespace
    names, not including the leading namespace separator, in the fully
    qualified class name (a "namespace prefix") corresponds to at least one
    "base directory".

- The contiguous sub-namespace names after the "namespace prefix"
    correspond to a subdirectory within a "base directory", in which the
    namespace separators represent directory separators. The subdirectory
    name must match the case of the sub-namespace names.

- The terminating class name corresponds to a file name ending in `.php`.
    The file name must match the case of the terminating class name.

- Autoloader implementations must not throw exceptions, must not raise errors
    of any level, and should not return a value.
 
## 2.3 Files

All PHP files must use the Unix LF (linefeed) line ending only.

All PHP files must end with a non-blank line, terminated with a single LF.

The closing `?>` tag must be omitted from files containing only PHP.

PHP code must use the long `<?php ?>` tags, it must not use the other tag variations.

PHP code must use only UTF-8 without BOM.

### 2.3.1 Lines

There must not be a hard limit on line length.

The soft limit on line length must be 120 characters.

There must not be trailing whitespace at the end of lines.

Blank lines may be added to improve readability and to indicate related
blocks of code except where explicitly forbidden.

There must not be more than one statement (statement ends with a semicolon) per line.

### 2.3.2 Indenting

Code must use an indent of 4 spaces for each indent level, and must not use
tabs for indenting.

### 2.3.3 Side Effects

A file should declare new symbols (classes, functions, constants,
etc.) and cause no other side effects, or it should execute logic with side
effects, but should not do both.

The phrase "side effects" means execution of logic not directly related to
declaring classes, functions, constants, etc., *merely from including the
file*.

"Side effects" include but are not limited to: generating output, explicit
use of `require` or `include`, connecting to external services, modifying ini
settings, emitting errors or exceptions, modifying global or static variables,
reading from or writing to a file, and so on.

The following is an example of a file with both declarations and side effects;
i.e, an example of what to avoid:

~~~php
<?php
// side effect: change ini settings
ini_set('error_reporting', E_ALL);

// side effect: loads a file
include 'file.php';

// side effect: generates output
echo '<html>';

// declaration
function foo() {
    // function body
}
~~~

The following example is of a file that contains declarations without side
effects; i.e., an example of what to emulate:

~~~php
<?php
// declaration
function foo() {
    // function body
}

// conditional declaration is *not* a side effect
if (!function_exists('bar')) {
    function bar() {
        // function body
    }
}
~~~

## 2.4. Namespace and Class Names

This means each class should be in a file by itself, and is in a namespace of at
least one level: a top-level vendor name.
Exceptions can be made for small utility classes which are only used in the same file.

Class names must be declared in `PascalCase`.

Code must use formal namespaces.

For example:

~~~php
<?php
namespace Vendor\Model;

class Foo {
}
~~~

## 2.5. Class Constants, Properties, and Methods

The term "class" refers to all classes, interfaces, and traits.

### 2.5.1. Constants

Class constants must be declared in all upper case with underscore separators.
For example:

~~~php
<?php
namespace Vendor\Model;

class Foo {
    const VERSION = '1.0';
    const DATE_APPROVED = '2012-06-01';
}
~~~

### 2.5.2. Properties

Whatever naming convention is used should be applied consistently within a
reasonable scope. That scope may be vendor-level, package-level, class-level,
or method-level but `camelCase` should be preferred.

### 2.5.3. Methods

Method names must be declared in `camelCase()`, unless overriding a method of the parent class.

### 2.5.4. Keywords and Types

All PHP reserved keywords and types [[1]][keywords][[2]][types] must be in lower case.

Any new types and keywords added to future PHP versions must be in lower case.

Short form of type keywords must be used i.e. `bool` instead of `boolean`,
`int` instead of `integer` etc.

Using types is preferred but not required. Use with caution!

~~~php
function fooBar(string $foo): string {
    return sprintf('%sBar', $foo);
}

// nullable string
function fooBarBaz(?string $foo): ?string {
    if ($foo) {
        return sprintf('%sBarBaz', $foo);
    }
    
    return null;
}
~~~

~~~php
// nullable string staring from PHP 8
function fooBarBaz(string|null $foo): string|null {
    if ($foo) {
        return sprintf('%sBarBaz', $foo);
    }
    
    return null;
}
~~~

## 3. Declare Statements, Namespace, and Import Statements

The header of a PHP file may consist of a number of different blocks. If present,
each of the blocks below must be separated by a single blank line, and must not contain
a blank line. Each block must be in the order listed below, although blocks that are
not relevant may be omitted.

* Opening `<?php` tag.
* File-level docblock.
* One or more declare statements.
* The namespace declaration of the file.
* One or more class-based `use` import statements.
* One or more function-based `use` import statements.
* One or more constant-based `use` import statements.
* The remainder of the code in the file.

When a file contains a mix of HTML and PHP, any of the above sections may still
be used. If so, they must be present at the top of the file, even if the
remainder of the code consists of a closing PHP tag and then a mixture of HTML and
PHP.

When the opening `<?php` tag is on the first line of the file, it must be on its
own line with no other statements unless it is a file containing markup outside of PHP
opening and closing tags.

Import statements must never begin with a leading backslash as they
must always be fully qualified.

Import statements should be logically ordered and grouped.

The following example illustrates a complete list of all blocks:

~~~php
<?php

/**
 * This file contains an example of coding styles.
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
 * FooBar is an example class.
 */
class FooBar {
    // ... additional PHP code ...
}

~~~

Compound namespaces with a depth of more than two must not be used. Therefore the
following is the maximum compounding depth allowed:
~~~php
<?php

use Vendor\Package\SomeNamespace\{
    SubnamespaceOne\ClassA,
    SubnamespaceOne\ClassB,
    SubnamespaceTwo\ClassY,
    ClassZ,
};
~~~

And the following would not be allowed:

~~~php
<?php

use Vendor\Package\SomeNamespace\{
    SubnamespaceOne\AnotherNamespace\ClassA,
    SubnamespaceOne\ClassB,
    ClassZ,
};
~~~

When wishing to declare strict types in files containing markup outside PHP
opening and closing tags, the declaration must be on the first line of the file
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

Declare statements must contain no spaces and must be exactly `declare(strict_types=1)`
(with an optional semi-colon terminator).

Block declare statements are allowed and must be formatted as below. Note position of
braces and spacing:
~~~php
declare(ticks=1) {
    // some code
}
~~~

## 4. Classes, Properties, and Methods

The term "class" refers to all classes, interfaces, and traits.

Any closing brace must not be followed by any comment or statement on the
same line.

When instantiating a new class, parentheses must always be present even when
there are no arguments passed to the constructor.

~~~php
new Foo();
~~~

### 4.1 Extends and Implements

The `extends` and `implements` keywords must be declared on the same line as
the class name.

The opening brace for the class must go on the same line; the closing brace
for the class must go on the next line after the body.

Opening braces must be on the same line and must not be preceded or followed
by a blank line.

Closing braces must be on their own line and must not be preceded by a blank
line.

~~~php
<?php

namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class ClassName extends ParentClass implements \ArrayAccess, \Countable {
    // constants, properties, methods
}
~~~

Lists of `implements` and, in the case of interfaces, `extends` may be split
across multiple lines, where each subsequent line is indented once. When doing
so, the first item in the list must be on the next line, and there must be only
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
    \Serializable {
    // constants, properties, methods
}
~~~

### 4.2 Using traits

The `use` keyword used inside the classes to implement traits must be
declared on the next line after the opening brace.

~~~php
<?php

namespace Vendor\Package;

use Vendor\Package\FirstTrait;

class ClassName {
    use FirstTrait;
}
~~~

Each individual trait that is imported into a class must be included
one-per-line and each inclusion must have its own `use` import statement.

~~~php
<?php

namespace Vendor\Package;

use Vendor\Package\FirstTrait;
use Vendor\Package\SecondTrait;
use Vendor\Package\ThirdTrait;

class ClassName {
    use FirstTrait;
    use SecondTrait;
    use ThirdTrait;
}
~~~

When the class has nothing after the `use` import statement, the class
closing brace must be on the next line after the `use` import statement.

~~~php
<?php

namespace Vendor\Package;

use Vendor\Package\FirstTrait;

class ClassName {
    use FirstTrait;
}
~~~

Otherwise, it must have a blank line after the `use` import statement.

~~~php
<?php

namespace Vendor\Package;

use Vendor\Package\FirstTrait;

class ClassName {
    use FirstTrait;

    private $property;
}
~~~

When using the `insteadof` and `as` operators they must be used as follows taking
note of indentation, spacing, and new lines.

~~~php
<?php

class Talker {
    use A;
    use B {
        A::smallTalk insteadof B;
    }
    use C {
        B::bigTalk insteadof C;
        C::mediumTalk as FooBar;
    }
}
~~~

### 4.3 Properties and Constants

Visibility must be declared on all properties and constants.

The `var` keyword must not be used to declare a property.

There must not be more than one property declared per statement.

Property names must not be prefixed with a single underscore to indicate
protected or private visibility. That is, an underscore prefix explicitly has
no meaning.

There must be a space between type declaration and property name.

A property declaration looks like the following:

Static class properties should be declared before non-static properties.

~~~php
<?php

namespace Vendor\Package;

class ClassName {
    public static int $bar = 0;
    public $foo = null;
}
~~~

### 4.4 Methods and Functions

Visibility must be declared on all methods.

Method names must not be prefixed with a single underscore to indicate
protected or private visibility. That is, an underscore prefix explicitly has
no meaning.

Method and function names must not be declared with space after the method name. The
opening brace must go on the same line, and the closing brace must go on the
next line following the body. There must not be a space after the opening
parenthesis, and there must not be a space before the closing parenthesis.

Static class methods should be declared after non-static methods.

A method declaration looks like the following. Note the placement of
parentheses, commas, spaces, and braces:

~~~php
<?php

namespace Vendor\Package;

class ClassName {
    public function fooBarBaz($arg1, &$arg2, $arg3 = []) {
        // method body
    }
    
    public static function fooBar($arg1, &$arg2, $arg3 = []) {
        // method body
    }
}
~~~

A function declaration looks like the following. Note the placement of
parentheses, commas, spaces, and braces:

~~~php
<?php

function fooBarBaz($arg1, &$arg2, $arg3 = []) {
    // function body
}
~~~

### 4.5 Method and Function Arguments

In the argument list, there must not be a space before each comma, and there
must be one space after each comma.

Method and function arguments with default values must go at the end of the argument
list.

~~~php
<?php

namespace Vendor\Package;

class ClassName {
    public function foo(int $arg1, &$arg2, $arg3 = []) {
        // method body
    }
}
~~~

Argument lists may be split across multiple lines, where each subsequent line
is indented once. When doing so, the first item in the list must be on the
next line, and there must be only one argument per line.

When the argument list is split across multiple lines, the closing parenthesis
and opening brace must be placed together on their own line with one space
between them.

~~~php
<?php

namespace Vendor\Package;

class ClassName {
    public function aVeryLongMethodName(
        ClassTypeHint $arg1,
        &$arg2,
        array $arg3 = []
    ) {
        // method body
    }
}
~~~

When you have a return type declaration present, there must be one space after
the colon followed by the type declaration. The colon and declaration must be
on the same line as the argument list closing parenthesis with no spaces between
the two characters.

~~~php
<?php

declare(strict_types=1);

namespace Vendor\Package;

class ReturnTypeVariations {
    public function functionName(int $arg1, $arg2): string {
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

In nullable type declarations, there must not be a space between the question mark
and the type.

~~~php
<?php

declare(strict_types=1);

namespace Vendor\Package;

class ReturnTypeVariations {
    public function functionName(?string $arg1, ?int &$arg2): ?string {
        return 'foo';
    }
}
~~~

When using the reference operator `&` before an argument, there must not be
a space after it, like in the previous example.

There must not be a space between the variadic three dot operator and the argument
name:

```php
public function process(string $algorithm, ...$parts) {
    // processing
}
```

When combining both the reference operator and the variadic three dot operator,
there must not be any space between the two of them:

```php
public function process(string $algorithm, &...$parts) {
    // processing
}
```

### 4.6 `abstract`, `final`, and `static`

When present, the `abstract` and `final` declarations must precede the
visibility declaration.

When present, the `static` declaration must come after the visibility
declaration.

~~~php
<?php

namespace Vendor\Package;

abstract class ClassName {
    protected static $foo;

    abstract protected function zim();

    final public static function bar() {
        // method body
    }
}
~~~

### 4.7 Method and Function Calls

When making a method or function call, there must not be a space between the
method or function name and the opening parenthesis, there must not be a space
after the opening parenthesis, and there must not be a space before the
closing parenthesis. In the argument list, there must not be a space before
each comma, and there must be one space after each comma.

~~~php
<?php

bar();
$foo->bar($arg1);
Foo::bar($arg2, $arg3);
~~~

Argument lists may be split across multiple lines, where each subsequent line
is indented once. When doing so, the first item in the list must be on the
next line, and there must be only one argument per line. A single argument (except for anonymous function) being
split across multiple lines (as might be the case with an anonymous function or
array) constitutes splitting the argument list itself.

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

somefunction(function () {
    // ...
}, $foo, $bar);

somefunction(
    $foo,
    $bar,
    [
      // ...
    ],
    $baz
);

$app->get('/hello/{name}', function ($name) use ($app) {
    return 'Hello ' . $app->escape($name);
});
~~~

## 5. Control Structures

The general style rules for control structures are as follows:

- There must be one space after the control structure keyword
- There must not be a space after the opening parenthesis
- There must not be a space before the closing parenthesis
- There must be one space between the closing parenthesis and the opening
  brace
- The structure body must be indented once
- The body must be on the next line after the opening brace
- The closing brace must be on the next line after the body

The body of each structure must be enclosed by braces. This standardizes how
the structures look and reduces the likelihood of introducing errors as new
lines get added to the body.

### 5.1 `if`, `else if`, `else`

An `if` structure looks like the following. Note the placement of parentheses,
spaces, and braces; and that `else` and `else if` are on the same line as the
closing brace from the earlier body.

~~~php
<?php

if ($expr1) {
    // if body
} else if ($expr2) {
    // else if body
} else {
    // else body;
}
~~~

The keyword `else if` should be used instead of `elseif`.

Expressions in parentheses may be split across multiple lines, where each
subsequent line is indented at least once. When doing so, the first condition
must be on the next line. The closing parenthesis and opening brace must be
placed together on their own line with one space between them. Boolean
operators between conditions must always be at the beginning or at the end of
the line, not a mix of both.

~~~php
<?php

if (
    $expr1
    && ($expr2 || $expr3)
    && $expr4
) {
    // if body
} else if (
    $expr1
    && $expr2
) {
    // else if body
}
~~~

### 5.2 `switch`, `case`

A `switch` structure looks like the following. Note the placement of
parentheses, spaces, and braces. The `case` statement must be indented once
from `switch`, and the `break` keyword (or other terminating keywords) must be
indented at the same level as the `case` body. There must be a comment such as
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

Expressions in parentheses may be split across multiple lines, where each
subsequent line is indented at least once. When doing so, the first condition
must be on the next line. The closing parenthesis and opening brace must be
placed together on their own line with one space between them. Boolean
operators between conditions must always be at the beginning or at the end of
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

Expressions in parentheses may be split across multiple lines, where each
subsequent line is indented at least once. When doing so, the first condition
must be on the next line. The closing parenthesis and opening brace must be
placed together on their own line with one space between them. Boolean
operators between conditions must always be at the beginning or at the end of
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

Expressions in parentheses may be split across multiple lines, where each
subsequent line is indented at least once. When doing so, the first condition
must be on the next line. Boolean operators between conditions must
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

Expressions in parentheses may be split across multiple lines, where each
subsequent line is indented at least once. When doing so, the first expression
must be on the next line. The closing parenthesis and opening brace must be
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

### 5.6 `try`, `catch`, `finally`

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

## 6. Strings

Use single quotes '' for strings or "" for string templates.

Strings that cause the line to go over 100 characters should not be written across multiple lines using string concatenation.

~~~php
$errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';
~~~

### 6.1 String interpolation

Prefer sprintf/printf or string template "" for string interpolation.

~~~php
$text = sprintf('Hello %s, welcome to our class %s.', $name, $className);
~~~

~~~php
$text = "Hello $name, welcome to our class $className.";
~~~

Use dot operator only for single concatenation.

~~~php
$price = $price . '€';
~~~

## 7. Operators

Style rules for operators are grouped by arity (the number of operands they take).

When space is permitted around an operator, multiple spaces may be
used for readability purposes.

All operators not described here are left undefined.

### 7.1. Unary operators

The increment/decrement operators must not have any space between
the operator and operand.
~~~php
$i++;
++$j;
~~~

Type casting operators must not have any space within the parentheses:
~~~php
$intValue = (int) $input;
~~~

### 7.2. Binary operators

All binary [arithmetic][], [comparison][], [assignment][], [bitwise][],
[logical][], [string][], and [type][] operators must be preceded and
followed by at least one space:
~~~php
if ($a === $b) {
    $foo = $bar ?? $a ?? $b;
} else if ($a > $b) {
    $foo = $a + $b * $c;
}
~~~

### 7.3. Ternary operators

The conditional operator, also known simply as the ternary operator, must be
preceded and followed by at least one space around both the `?`
and `:` characters:
~~~php
$variable = $foo ? 'foo' : 'bar';
~~~

When the middle operand of the conditional operator is omitted, the operator
must follow the same style rules as other binary [comparison][] operators:
~~~php
$variable = $foo ?: 'bar';
~~~

## 8. Closures

Closures must be declared with a space after the `function` keyword, and a
space before and after the `use` keyword.

The opening brace must go on the same line, and the closing brace must go on
the next line following the body.

There must not be a space after the opening parenthesis of the argument list
or variable list, and there must not be a space before the closing parenthesis
of the argument list or variable list.

In the argument list and variable list, there must not be a space before each
comma, and there must be one space after each comma.

Closure arguments with default values must go at the end of the argument
list.

If a return type is present, it must follow the same rules as with normal
functions and methods; if the `use` keyword is present, the colon must follow
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

Argument lists and variable lists may be split across multiple lines, where
each subsequent line is indented once. When doing so, the first item in the
list must be on the next line, and there must be only one argument or variable
per line.

When the ending list (whether of arguments or variables) is split across
multiple lines, the closing parenthesis and opening brace must be placed
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

## 9. Anonymous Classes

Anonymous Classes must follow the same guidelines and principles as closures
in the above section.

~~~php
<?php

$instance = new class {};
~~~

The opening brace may be on the same line as the `class` keyword so long as
the list of `implements` interfaces does not wrap. If the list of interfaces
wraps, the brace must be placed on the same line following the last
interface.

~~~php
<?php

// Brace on the same line
$instance = new class extends \Foo implements \HandleableInterface {
    // Class content
};

$instance = new class extends \Foo implements
    \ArrayAccess,
    \Countable,
    \Serializable {
    // Class content
};
~~~

## 10. Comments

Use /** ... */ for multiline comments.

Use // for single line comments. Place single line comments on a newline above the subject of the comment. Put an empty line before the comment unless it’s on the first line of a block.

Start all comments with a space to make it easier to read.

~~~php
// fooBarBaz

/**
 * foo
 * bar
 * baz
 */
~~~

Prefer meaningful and pronounceable names over comments.

~~~php
// add body classes
add_filter('body_class', [$this, 'bodyClassFilter']);
~~~

~~~php
// class FooBar
class FooBar {
    /**
     * Current object ID
     *
     * @var int|string|null
     */
    public $IO;
}
~~~

~~~php
add_filter('body_class', [$this, 'addBodyClasses']);
~~~

~~~php
class FooBar {
    public int|string|null $IO;
}
~~~

## 10.1 TODO and FIXME

Prefixing your comments with FIXME or TODO helps other developers quickly understand if you’re pointing out a problem that needs to be revisited, or if you’re suggesting a solution to the problem that needs to be implemented. These are different than regular comments because they are actionable. The actions are FIXME: -- need to figure this out or TODO: -- need to implement.

Use // FIXME: to annotate problems.
~~~php
// FIXME: shouldn’t use a global here
global $total;

$total = 0;
~~~

Use // TODO: to annotate solutions to problems.
~~~php
class Calculator {
    private $total;

    public function __constructor() {
        // TODO: total should be configurable by an options param
        $this->total = 0;
    }
}
~~~

## Resources
- [PHP Manual][PHP]
- [PHP Standards Recommendations][PSR]
- [PHP Clean Code][clean-code]

[PSR]: https://www.php-fig.org/psr/
[PHP]: https://www.php.net/manual/en/
[PSR-12]: https://www.php-fig.org/psr/psr-12/
[PSR-1]: https://www.php-fig.org/psr/psr-1/
[PSR-2]: https://www.php-fig.org/psr/psr-2/
[keywords]: http://php.net/manual/en/reserved.keywords.php
[types]: http://php.net/manual/en/reserved.other-reserved-words.php
[arithmetic]: http://php.net/manual/en/language.operators.arithmetic.php
[assignment]: http://php.net/manual/en/language.operators.assignment.php
[comparison]: http://php.net/manual/en/language.operators.comparison.php
[bitwise]: http://php.net/manual/en/language.operators.bitwise.php
[logical]: http://php.net/manual/en/language.operators.logical.php
[string]: http://php.net/manual/en/language.operators.string.php
[type]: http://php.net/manual/en/language.operators.type.php
[clean-code]: https://github.com/jupeter/clean-code-php
