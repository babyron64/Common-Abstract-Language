# Index
- [Create object](#create-object)
- [Object type](#object-type)
    - [Literal object](#literal-object)
    - [Expression object](#expression-object)
    - [Value object](#value-object)
    - [Transient object](#transient-object)
    - [Lazy object](#lazy-object)
    - [Symbol object](#symbol-object)
- [Function](#function)
    - [Bound](#bound)
    - [Free](#free)
- [Substitution](#substitution)
    - [Normal substitution](#normal-substitution)
    - [Constant substitution](#constant-substitution)
- [Context application](#context-application)
- [Access control](#access-control)
- [Forced evaluation](#forced-evaluation)
- [Comment](#comment)
- [None value](#none-value)

<a id = "create-object"></a>
# Create object
Usage:
```
>> *** // definition
```

Example:
```
>> 1 :: literal object
>> (1 + 1) :: value object
>> [1 + 1] :: transient object
>> {1 + 1} :: lazy object
>> :x :: symbol object
```

<a id = "object-type"></a>
# Object
Usage:
```
>> // => [context]object
```

<a id = "literal-object"></a>
## 1. Literal object (literal)
Types:
- int
- float
- char
- list
- (string)
- none

Usage:
```
>> literal object
```

Example:
```
>> 1 :: int  // => 1 :: int
>> 3.14 :: float  // => 3.14 :: float
>> "a" :: char  // => "a" :: char
>> `(1, 2, 3) :: list  // => `(1, 2, 3) :: list
>> `("a", "b", "c") :: list  // => `("a", "b", "c") :: list = string
>> `(1, "a", "abc") :: list  // => `(1, "a", "abc") :: list
>> "abc" :: string  // => `("a", "b", "c") :: string
>> _ :: none  // => _ :: none

>> $1  // => 1
>> $"a"  // => "a"
>> $`(1, 2, 3)  // => `(1, 2, 3)
>> $_  // => _
```

<a id = "expression-object"></a>
## 2. Expression object (expr)
Usage:
```
>> expr
>> expr, expr, ..., expr
>> expr; expr
```

Example:
```
>> :: none  // => _ :: none
>> _ :: none  // => _ :: none

>> 1 + 1 :: expr<int + int>  // => 2 :: int
>> 0.1 + 0.1 :: expr<float + float>  // => 0.2 :: float
>> `(1, 2) + `(3, 4) :: expr<list + list>  // => `(1, 2, 3, 4) :: list

>> `(1, 2) + 3 :: expr<list + int>  // => `(1, 2, 3) :: list
>> "ab" + "c" :: expr<string + char>  // => "abc" :: string
>> "a" + 1 :: expr<char + int>  // => "a1" :: string
>> "ab" + 1 :: expr<string + int>  // => "ab1"

>> `(1, 2, 3).0 :: expr<list.so>  // => 1
>> "abcd".2 :: expr<string.so>  // => "c"

>> obj = {x} :: expr<so = lo>  // => _ :: none
>> obj.x = 1 :: expr<expr<lo.so = int>  // => _ :: none
>> obj.x :: expr<lo.so>  // => 1 :: int

>> x = 1 + 1  // x = 2  => _
>> x  // => 2
>> $x  // => 2

>> obj = x  // obj = expr
>> $obj  // => _  => _
>> obj.x = 1
>> obj  // => 1

>> 1, 2, 3  // => 3
>> 1 + 1, 2 + 2, 3 + 3  // => 2, 4, 6  => 6

>> 1, 2; 3  // => 2
```

<a id = "value-object"></a>
## 3. Value object (vo)
Usage:
```
>> (expr)
>> (
>|     expr
>| )
```

Example:
```
>> () :: (none)  // => _ :: none
>> (1) :: (int)  // => 1 :: int
>> (1 + 1) :: (expr<int + int>)  // => 2 :: int
>> ((1 + 1)) :: ((expr<int + int>))  // => 2 :: int
>> (1 + 1) * (1 + 1) :: expr<vo * vo>  // => 4 :: int

>> x = (1)  // x = 1  => _
>> x  // => 1
>> $x  // => 1

>> x = ((1))  // x = 1
>> x  // => 1

>> x = 1
>> obj = (x + 1)  // obj = [x = 1](x + 1)  => 2
>> obj  // => 2
```

<a id = "transient-object"></a>
## 4. Transient object (to)
Usage:
```
>> [expr]
>> [
>|     expr
>| ]
```

Example:
```
>> [] :: [none]  // => [] :: [none]
>> [1] :: [int]  // => [1] :: [int]
>> [1 + 1] :: [expr<int + int>]  // => [1 + 1] :: [[expr<int + int>]]
>> [[1 + 1]] :: [[expr<int + int>]]  // => [[1 + 1]] :: [[expr<int + int>]]
>> [1 + 1] * [1 + 1] :: expr<to * to>  // => (E) invalid operand

>> x = [1]  // x = [1]  => _
>> x  // => (1)  // => 1
>> $x  // => (1)  // => 1

>> x = [[1]]  // x = [[1]]
>> x  // => ([1])  => [1]

>> x = 1
>> obj = [x + 1]  // obj = [x = 1][x + 1]
>> obj  // => [x = 1](x + 1)  => 2
```

<a id = "lazy-object"></a>
## 5. Lazy object (lo)
Usage:
```
>> {expr}
>> {
>|     expr
>| }
```

Example:
```
>> {} :: {none}  // => {} :: {none}
>> {1} :: {int}  // => {1} :: {int}
>> {1 + 1} :: {expr<int + int>}  // => {1 + 1} :: {expr<int + int>}
>> {{1 + 1}} :: {{expr<int + int>}}  // => {{1 + 1}} :: {{expr<int + int}}
>> {1 + 1} * {1 + 1} :: expr<lo * lo>  // => (E) invalid operand

>> x = {1}  // => _
>> x  // => {1}
>> $x  // => (1)  // => 1

>> x = {{1}}  // x = {{1}}
>> x  // => {{1}}

>> x = 1
>> obj = {x + 1}  // obj = [x = 1]{x + 1}
```

<a id = "symbol-object"></a>
## 6. Symbol object (so)
Usage:
```
>> :symbol
>> :char
>> :string
>> symbol => so
>> &symbol = object
```

Example:
```
>> :x :: so
>> :_  // => (E) invalid name of a symbol

>> x = 1
>> :x  // => :x
>> $:x  // => 1

>> x = :y
>> x  // => :y
>> $x  // => _

>> x = 1
>> y = :x
>> y  // => :x
>> $y  // => x => 1
>> x = 2
>> $y  // => x => 2

>> :"x"  // => :x
>> :"xyz"  // => :xyz

>> a = 1
>> x = "a"
>> y = :x
>> z = :(x)
>> $y  // => x  => "a"
>> $z  // => a  => 1

// NOTE: y => x = 1
>> x = 1
>> y => x
>> y  // => x => 1
>> x = 2
>> y  // => x => 2
>> y = 1
>> x  // => 2
>> y  // => 1

>> x = 1
>> y => x
>> z => y
>> &z = 2
>> z  // => 2
>> y  // => 2
>> x  // => 2
```

<a id = "function"></a>
# Function
Usage:
```
>> param-symbol -> object
```

Example:
```
>> x -> x :: \x. expr
>> x -> (x) :: \x. vo
>> x -> y -> [x + y] :: \x, y. [expr]
>> x -> y -> {x} :: \x, y. {expr}

>> (x -> x + 1) 1  // => 2
>> [x -> x + 1] 1  // => (E) too many arguments

>> f = x -> x + 1
>> f 1  // => [x = 1]\x. x + 1  => 1 + 1  => 2
>> g = x -> y -> (x * 10) + y
>> g 1 2  // => [x = 1, y = 2]\. (x * 10) + y  => (1 * 10) + 2  => 12
>> g 1  // => [x = 1]\y. (x * 10) + y
>> h = g 1
>> h 2  // => [x = 1, y = 2]\. (x * 10) + y  => (1 * 10) + 2  => 12

>> f = x -> y -> x + y
>> g = f 1  // g = [x = 1]\y . x + y
>> $g  // => [x = 1, y = _]\. x + y  => 1 + _  => _
>> g.y = 1  // g = [x = 1, y = 1]\y. x + y
>> $g  // => [x = 1, y = 1]\. x + y  => 1 + 1  => 2
>> g.x = 2  // g = [x = 2, y = 1]\y. x + y
>> $g  // => [x = 2, y = 1]\. x + y  => 2 + 1  => 3

>> f = x -> 1
>> f 2  // => 1
>> $f  // => 1
```

<a id = "bound"></a>
## Bound
Usage:
```
>> param-symbol -> object
```

Example:
```
>> f = x -> x + 1
>> f 1 // => 2

>> obj = x + 1
>> f = x -> obj
>> f 1  // => 2
```

<a id = "free"></a>
## Free
Usage:
```
>> param-symbol >- object
```

Example:
```
>> f = x -> x + 1
>> obj = x >- f  // obj = x + 1
>> obj  // => _ + 1  => _
>> obj.x = 1
>> obj  // => 1 + 1  => 2
```

<!--
<a id = "function-misc"></a>
## Misc
What if `:x -> obj` == `x -> obj` (syntax sugar):
```
>> x = :y
>> y = 1
>> f = :x -> x
>> f x  // => f :y  => :y
>> g = x -> x
>> g x  // => g :y or g :x ?
```
-->

<a id = "substitution"></a>
# Substitution

<a id = "normal-substitution"></a>
## Normal substitution
Usage:
```
>> symbol = object
```

Example:
```
>> x = 1
>> x  // => 1

>> x = 1 + 1
>> x  // => 2
```

<a id = "constant-substitution"></a>
## Constant substitution
Usage:
```
>> x := 1
```

Example:
```
>> x := 1
>> x = 2  // (E) access denied
>> obj = {x = 2}
>> $obj  // (E) access denied
```

<a id = "context-application"></a>
# Context application
Example:
```
>> x = 1
>> obj = {}
>> obj.x  // => 1
>> x = 2
>> obj.x  // => 2
>> obj.x = 3
>> x  // => 2
>> x = 1
>> obj.x  // => 3
```

<a id = "access-control"></a>
# Access control
Usage:
```
*public*
>> @+symbol
*protected*
>> @#symbol
*private*
>> @-symbol
```

Example:
```
>> x = 1
>> @+y = 2
>> @#z = 3
>> @-w = 4
>> x  // => 1
>> y  // => 2
>> z  // => 3
>> w  // => 4
>> obj = {a = 1, @+b = 2, @#c = 3, @-d = 4}
>> obj.x  // => 1
>> obj.y  // => 2
>> obj.z  // => (E) access denied
>> obj.w  // => _
>> obj.a  // => 1
>> obj.b  // => 2
>> obj.c  // => (E) access denied
>> obj.d  // => (E) access denied
>> @+w
>> obj = {a = 1, @+b = 2, @#c = 3, @-d = 4}
>> obj.w  // => 4
```

<a id = "forced-evaluation"></a>
# Forced evaluation
Usage:
```
>> $symbol
>> $object
```

Example:
```
>> $1 + 1  // == ($1) + 1
>> $(1 + 1)  // => 2
>> $[1 + 1]  // => 2
>> ${1 + 1}  // => 2
>> ${{1 + 1}}  // => {1 + 1}

>> x = {{1 + 1}}
>> $x  // => {1 + 1}
```

<a id = "comment"></a>
# Comment
Usage:
```
>> ~~~ // comment
>> ~~~ /* comment
>> */
>> ~~~ /* comment */
```

Example:
```
>> 1 + 1 // one plus one
>> 1 + 1 /* one plus one */
>> 1 + 1 /*
>| one plus one
>| */
```

<a id = "none-value"></a>
# None value
Usage:
```
>> _  // => _
```

Example:
```
>> _  // => _
>> $_  // => _
>> _ = 1
>> _  // = > _
>> _.x  // => _
>> _ 1 2  // => _

>> obj = _
>> obj.x = 1
>> obj.x  // => _
>> (x -> obj) 1  // => _
```