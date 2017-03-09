# Index
- [Keyword](#keyword)
- [If](#if)
- [Switch](#switch)
- [While](#while)
- [For](#for)
- [Foreach](#foreach)

<a id = "syntax"></a>
# Syntax (example)

<a id = "keyword"></a>
## 1. Keyword
When you define a keyword (reserved word), follow this section. The language has two kinds of substitution methods – normal, constant. In this method, you use constant substitution. All that you have to do is define a symbol refers to the same symbol. For example,

```
>> var := :var
```

You cannot re-use symbol `var` for any other purpose than a keyword because `var` is constantly substituted. And no matter how many times you evaluate the `var`, it returns `:var`.

```
>> var      => :var
>> $$var    => $$(:var) => $(var) => $(:var) => var => :var
```

<a id = "if"></a>
## 2. If
Usage:
```
if cond {when true} else {when false}
```

Definition:
```
>> true := x -> y -> x
>> false := x -> y -> y

*system defined*
>> a == b := if (a == b) then {$:true} else {$:false}
>> a != b := not a == b

>> bool? := val -> (
>|     (val != true && val != false) false true
>| )

>> else := :else

>> if := cond -> t_block -> _else -> f_block -> (
>|     (bool? cond) [cond [$t_block] [$f_block]] _
>| )
```

<a id = "switch"></a>
## 3. Switch
Usage:
```
switch val {
    case val1 {
        
    },
    case val2 {

    },
    ...
}
```

Definition:
```
>> case := _val -> _block -> (
>|     [_cases.add {val = _val, block = _block}]
>| )

>> switch := val -> block -> (
>|     block._cases = list `() ,
>|     $block ,
>|     t_case = block._cases.first [:obj -> (obj.val == val)] ,
>|     $t_case.block
>| )
```

Example
```
>> x = "zero"
>> switch 3 {
>|    case 1 {
>|        &x = "one"
>|    },
>|    case 2 {
>|        &x = "two"
>|    },
>|    case 3 {
>|        &x = "three"
>|    },
>|    case 4 {
>|        &x = "four"
>|    }
>| }
>> x  // => "three"
````

<a id = "while"></a>
## 4. While
Usage:
```
while cond block
```

Definition:
```
*loop type*
>> while := cond -> block -> (
>|     loop = (bool? cond) [$block, [loop]] _ ,
>|     loop
>| )

*recursive type*
>> while := cond -> block -> (
>|     loop = (bool? cond) [$block, loop] _ ,
>|     loop
>| )
```

Example:
```
>> x = 10
>> y = 0
>> while (x >= 0) {
>|     &x = x - 1 ,
>|     &y = y + x
>| }
>> y  // => 45
```

<a id = "for"></a>
## 5. For
Usage:
```
for :obj (cond) (procedure) {block}
```

Definition:
```
>> for := obj -> init -> cond -> each -> block -> (
>|     $obj = {} ,
>|     init.$obj => $obj ,
>|     cond.$obj => $obj ,
>|     each.$obj => $obj ,
>|     block.$obj => $obj ,
>|     while cond {
>|         $block
>|         each
>|     }
>| )
```

Example:
```
>> x = 0
>> for :i (i < 10) (i = i + 1) {
>|     &x = x + i
>| }
>> x  // => 45
```

<a id = "foreach"></a>
## 5. Foreach
Usage:
```
foreach :obj in iter {loop}
```

Definition:
```
>> in := :in    // keyword definition

>> foreach := obj -> _in -> iter -> block -> (   // use _in instead of in
>|     if ( _in != :in || symbolObj? obj ) _
>|     else {iter.each [$obj -> block]}
>| )
```

Example:
```
>> x = 0
>> foreach :obj in `(1, 2, 3) {
>|     &x = x + obj
>| }
>> x  // => 6
```