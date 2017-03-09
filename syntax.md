<a id = "syntax-definition"></a>
# Syntax definition (example)

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

<a id = "if-sentense"></a>
## 2. If sentense
Usage:
```
if cond {when true} else {when false}
```

Definition:
```
>> true := :x -> :y -> x
>> false := :x -> :y -> y

>> bool? := :val -> (
>|     (val != true && val != false) false true
>| )

>> else := :else

>> if := :cond -> :t_block -> :_else -> :f_block -> (
>|     (bool? cond) [cond [$t_block] [$f_block]] _
>| )
```

<a id = "while-sentense"></a>
## 3. While sentense
Usage:
```
while cond block
```

Definition:
```
*loop type*
>> while := :cond -> :block -> (
>|     loop = (bool? cond) [$block, [loop]] _
>|     loop
>| )

*recursive type*
>> while := :cond -> :block -> (
>|     loop = (bool? cond) [$block, loop] _
>|     loop
>| )
```

Example:
```
>> x = 10
>> y = 0
>> while (x >= 0) {
>|     &x = x - 1
>|     &y = y + x
>| }
>> y  // => 45
```

<a id = "for-sentense"></a>
## 4. For sentense
Usage:
```
for :obj in iter {loop}
```

Definition:
```
>> in := :in    // keyword definition

>> for := :obj -> :_in -> :iter -> :block -> (   // use :_in instead of :in
>|     if ( _in != :in || symbolObj? obj ) _ else [
>|         iter.each [obj -> block]
>|     ]
>| )
```

Example:
```
>> x = 0
>> for :obj in `(1, 2, 3) {
>|     &x = x + obj
>| }
>> x  // => 6
```